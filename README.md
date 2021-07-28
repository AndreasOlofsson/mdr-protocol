# MDR Protocol

This repo documents my attempts to reverse-engineer the sony headphones protocol (internally referred to as MDR), used by the [Sony Headphones Connect](https://play.google.com/store/apps/details?id=com.sony.songpal.mdr) app. This is what allows users to change certain settings on their headphones such as button actions, noise cancelling levels, equalizer, etc.

For an implementation of this protocol see [libmdr](https://github.com/AndreasOlofsson/libmdr) and [mdrd](https://github.com/AndreasOlofsson/mdrd).

See [reverse engineering](reverse_engineering.md) for how the protocol was reverse engineered or if you want to implement some part of the protocol not documented here.

This documentation is very incomplete and a work-in-progress, see the packet structs in [libmdr](https://github.com/AndreasOlofsson/libmdr) for a little more completeness.

## The Protocol

The app opens a RFCOMM connection on whichever channel provides a service UUID of `96CC203E-5068-46AD-B32D-E316F5E069BA`, this is different between my two devices (channel 9 for my WF-3000XM3, channel 15 for WH-3000XM3) that implement this protocol.

The devices then communicate by passing messages to each other in the following format:  
  `<0x3e> <frame> <0x3c>`.

Each frame is wrapped between a start byte with value `0x3e` (ascii `<`) and a stop byte with value `0x3c` (ascii `>`). The reader scans for the start byte, reads data until the stop byte and discards any other data. A frame may be longer than a single RFCOMM-packet and may be sent in multiple parts. Multiple frames may also be sent in a single RFCOMM-packet.

The frame between each set of start and stop bytes is escaped since the occurrence of a start or stop byte would indicate the start or end of a frame. The escaping is a simple substitution where is each escaped byte is prefixed with `0x3d` and has the 5th lowest bit cleared (binary AND `0xEF`). The escape byte itself is also escaped when present in the unescaped data.

Unescaping is performed by scanning for the escape byte (`0x3d`), skipping it and emitting the following byte binary OR'd with `0x10`.

i.e. this table is used:
| unescaped |   escaped   |
|:---------:|:-----------:|
|  `0x3c`   | `0x3d 0x2c` |
|  `0x3d`   | `0x3d 0x2d` |
|  `0x3e`   | `0x3d 0x2e` |

## Frames

Each frame contains a `DataType`, sequence ID, payload length, payload and a checksum in the following format:

|   offset   |  length   |             meaning              |       possible values       |
|:----------:|:---------:|:--------------------------------:|:---------------------------:|
|    0x00    |  1 byte   |            `DataType`            | see [DataTypes](#DataTypes) |
|    0x01    |  1 byte   |           sequence ID            |       `0x00` / `0x01`       |
|    0x02    |  4 bytes  | `N`: payload length (big endian) |                             |
|    0x03    | `N` bytes |             payload              |     depends on DataType     |
| 0x03 + `N` |  1 byte   |             checksum             |  see [Checksum](#Checksum)  |

### DataTypes

The following values determine what protocol the payload is to be interpeted as part of. The _Sone Headphones_ app only seems to use `DataMdr` and `DataMdrNo2` types.

|  byte  |         DataType          |
|:------:|:-------------------------:|
| `0x00` |           Data            |
| `0x01` |            Ack            |
| `0x02` |         DataMcNo1         |
| `0x09` |          DataIcd          |
| `0x0a` |          DataEv           |
| `0x0c` | [DataMdr](MDR_packets.md) |
| `0x0d` |        DataCommon         |
| `0x0e` |        DataMdrNo2         |
| `0x10` |           Shot            |
| `0x12` |         ShotMcNo1         |
| `0x19` |          ShotIcd          |
| `0x1a` |          ShotEv           |
| `0x1c` |          ShotMdr          |
| `0x1d` |        ShotCommon         |
| `0x1e` |        ShotMdrNo2         |
| `0x2d` |      LargeDataCommon      |

### Checksum

The checksum of a frame is a simple 1-byte wrapping addition of every other byte in the frame. 
