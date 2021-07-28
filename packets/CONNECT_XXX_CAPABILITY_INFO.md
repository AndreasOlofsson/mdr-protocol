# CONNECT_(GET/RET)_CAPABILITY_INFO

<br/>

## CONNECT_GET_CAPABILITY_INFO

| offset | length |      meaning       |
|:------:|:------:|:------------------:|
| `0x00` | 1 byte | fixed value `0x00` |

<br/>

## CONNECT_RET_CAPABILITY_INFO

| offset |        length         |      meaning       |
|:------:|:---------------------:|:------------------:|
| `0x00` |        1 byte         | fixed value `0x00` |
| `0x01` |        1 byte         | capability counter |
| `0x02` |        1 byte         | UUID length (`N`)  |
| `0x03` | `min(N, 128)` byte(s) |        UUID        |

The capability counter doesn't seem to have any significant use in the app other than verifying that it has a valid value.

The UUID is the the device's Bluetooth MAC address.

