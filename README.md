# MDR Protocol

This repo documents my attempts to reverse-engineer the sony headphones protocol (internally referred to as MDR), used by the [Sony Headphones Connect](https://play.google.com/store/apps/details?id=com.sony.songpal.mdr) app. This is what allows users to change certain settings on their headphones such as button actions, noise cancelling levels, equalizer, etc.

For an implementation of this protocol see [libmdr](https://github.com/AndreasOlofsson/libmdr) and [mdrd](https://github.com/AndreasOlofsson/mdrd).

See [reverse engineering](reverse_engineering.md) for how the protocol was reverse engineered or if you want to implement some part of the protocol not documented here.

This documentation is very incomplete and a work-in-progress, see the packet structs in [libmdr](https://github.com/AndreasOlofsson/libmdr) for a little more completeness.

## The Tandem Protocol

The MDR protocol is transmitted via another protocol which is called _Tandem_ (I think). Tandem is a simple wrapper around some other protocols and only provides synchronization and tags to indicate which sub-protocol each frame is to be interpreted as.

In this documentation __frames__ refer to _Tandem frames_ and __packets__ generally refer to _MDR packets_.

To start a connection the app opens a RFCOMM connection on whichever channel provides a service UUID of `96CC203E-5068-46AD-B32D-E316F5E069BA`. The channel number varies between devices (and possibly software versions) so using SDP is necessary to find the correct one (it's channel 9 for my WF-3000XM3 and 15 for my WH-3000XM3).

The devices then communicate by passing messages to each other in the following format:  
  `<0x3e> <frame-contents> <0x3c>`.

Each frame is wrapped between a start byte with value `0x3e` (ascii `<`) and a stop byte with value `0x3c` (ascii `>`). The receiver scans for the start byte, reads data until the stop byte and discards any other data.

A frame may be longer than a single RFCOMM-packet and may be sent in multiple parts. Multiple frames may also be sent in a single RFCOMM-packet. A single Tandem frame may also contain multiple MDR packets, I haven't observed MDR packets being split between multiple Tandem frames but it may also be possible. The best approach is probably to treat both RFCOMM packets and Tandem frames as containing a stream of data.

The frame contents between each set of start and stop bytes is escaped since the occurrence of a start or stop byte would indicate the start or end of a frame. The escaping is a simple substitution where is each escaped byte is prefixed with `0x3d` and has the 5th lowest bit cleared (binary AND `0xEF`). The escape byte itself is also escaped when present in the unescaped data.

Unescaping is performed by scanning for the escape byte (`0x3d`), skipping it and emitting the following byte binary OR'd with `0x10`.

i.e. this table is used:
| unescaped |   escaped   |
|:---------:|:-----------:|
|  `0x3c`   | `0x3d 0x2c` |
|  `0x3d`   | `0x3d 0x2d` |
|  `0x3e`   | `0x3d 0x2e` |

### Frames Contents

Each frame contains a `DataType`, sequence ID, payload length, payload and a checksum in the following format:

|   offset   |  length   |             meaning              |       possible values       |
|:----------:|:---------:|:--------------------------------:|:---------------------------:|
|    0x00    |  1 byte   |            `DataType`            | see [DataTypes](#DataTypes) |
|    0x01    |  1 byte   |           sequence ID            |       `0x00` / `0x01`       |
|    0x02    |  4 bytes  | `N`: payload length (big endian) |                             |
|    0x03    | `N` bytes |             payload              |     depends on DataType     |
| 0x03 + `N` |  1 byte   |             checksum             |  see [Checksum](#Checksum)  |

### DataTypes

The following values determine what protocol the payload is to be interpreted as part of. The only ones documented here are `Ack`, `DataMdr` which is used for most of the MDR protocol and `DataMdrNo2` which is used for some supplemental packets.

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

### Sequence ID

The _sequence ID_ is a simple value which alternates between `0x00` and `0x01` for each non-ACK frame sent. The other party should reply to each non-Ack frame with an Ack where the payload is empty and the sequence ID is set to the opposite of the received frame. If a frame is not Ack'd, it should be re-sent with the same sequence ID as before (i.e. sequence ID is only changed when an Ack is received).

An example session might look like this (only Ack/non-Ack and sequence ID shown):

|      A     |          |      B     |
|------------|:--------:|------------|
| Data 0     |          |            |
|            |  ----->  |            |
|            |          | Got Data 0 |
|            |          | Ack 1      |
|            |  <-----  |            |
| Got Ack 1  |          |            |
|            |   ....   |            |
|            |          | Data 0     |
|            |  <-----  |            |
| Got Data 0 |          |            |
| Ack 1      |          |            |
|            |  ----->  |            |
|            |          | Got Ack 1  |
|            |   ....   |            |
| Data 1     |          |            |
|            |  ----->  |            |
|            |          | Got Data 1 |
|            |          | Ack 0      |
|            |  <--/--  |            |
|            |  timeout |            |
| Data 1     |  ----->  |            |
|            |          | Got Data 1 |
|            |          | Ack 0      |
|            |  <-----  |            |
| Got Ack 0  |          |            |
|            |   ....   |            |
|            |          | Data 1     |
|            |  <-----  |            |
| Got Data 1 |          |            |
| Ack 0      |          |            |
|            |  ----->  |            |
|            |          | Got Ack 0  |

### Checksum

The checksum of a frame is a simple 1-byte wrapping addition of every other byte in the frame (unescaped). 

