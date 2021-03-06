# MDR Packets

The MDR protocol is mostly a request/response/notify protocol where the controller (app/computer/etc.) sends requests (GET packets or SET packets) and the device replies with a corresponding response packet (RET packet) or only an Ack. Some packets also have a notify (NTFY packet) variant which will be sent by the device any time some relevant value changes, as long as the controller has previously sent a corresponding GET packet.

Packet names have the form `[CATEGORY]_[GET/RET/SET/NTFY]_[PROPERTY]`.

The first packet sent by the controller should be [CONNECT_GET_PROTOCOL_INFO](packets/CONNECT_XXX_PROTOCOL_INFO.md) even if the information is not useful to the controller as some functions don't seem to work unless the device has received this packet first.

For some packets the response to [CONNECT_GET_SUPPORT_FUNCTION](packets/CONNECT_XXX_SUPPORT_FUNCTION.md) should be examined first to check that device supports that particular packet. Some values for some packets require checking the corresponding category's _GET_CAPABILITY_ response.

## Table

| byte | meaning                                                                 | description                                      |
|:----:|:------------------------------------------------------------------------|--------------------------------------------------|
| 0x00 | [CONNECT_GET_PROTOCOL_INFO](packets/CONNECT_XXX_PROTOCOL_INFO.md)       | Get the device's supported protocol version.     |
| 0x01 | [CONNECT_RET_PROTOCOL_INFO](packets/CONNECT_XXX_PROTOCOL_INFO.md)       |                                                  |
| 0x02 | [CONNECT_GET_CAPABILITY_INFO](packets/CONNECT_XXX_CAPABILITY_INFO.md)   |                                                  |
| 0x03 | [CONNECT_RET_CAPABILITY_INFO](packets/CONNECT_XXX_CAPABILITY_INFO.md)   |                                                  |
| 0x04 | [CONNECT_GET_DEVICE_INFO](packets/CONNECT_XXX_DEVICE_INFO.md)           | Get the device's model name, FW version, etc.    |
| 0x05 | [CONNECT_RET_DEVICE_INFO](packets/CONNECT_XXX_DEVICE_INFO.md)           |                                                  |
| 0x06 | [CONNECT_GET_SUPPORT_FUNCTION](packets/CONNECT_XXX_SUPPORT_FUNCTION.md) | Get a list of functions supported by the device. |
| 0x07 | [CONNECT_RET_SUPPORT_FUNCTION](packets/CONNECT_XXX_SUPPORT_FUNCTION.md) |                                                  |
| 0x10 | [COMMON_GET_BATTERY_LEVEL](packets/COMMON_XXX_BATTERY_LEVEL.md)         | Get the device's battery level.                  |
| 0x11 | [COMMON_RET_BATTERY_LEVEL](packets/COMMON_XXX_BATTERY_LEVEL.md)         |                                                  |
| 0x13 | [COMMON_NTFY_BATTERY_LEVEL](packets/COMMON_XXX_BATTERY_LEVEL.md)        |                                                  |
| 0x14 | COMMON_GET_UPSCALING_EFFECT                                             |                                                  |
| 0x15 | COMMON_RET_UPSCALING_EFFECT                                             |                                                  |
| 0x17 | COMMON_NTFY_UPSCALING_EFFECT                                            |                                                  |
| 0x18 | COMMON_GET_AUDIO_CODEC                                                  |                                                  |
| 0x19 | COMMON_RET_AUDIO_CODEC                                                  |                                                  |
| 0x1b | COMMON_NTFY_AUDIO_CODEC                                                 |                                                  |
| 0x1c | COMMON_GET_BLUETOOTH_DEVICE_INFO                                        |                                                  |
| 0x1d | COMMON_RET_BLUETOOTH_DEVICE_INFO                                        |                                                  |
| 0x22 | COMMON_SET_POWER_OFF                                                    | Turn the device off.                             |
| 0x24 | COMMON_GET_CONNECTION_STATUS                                            |                                                  |
| 0x25 | COMMON_RET_CONNECTION_STATUS                                            |                                                  |
| 0x27 | COMMON_NTFY_CONNECTION_STATUS                                           |                                                  |
| 0x28 | COMMON_GET_CONCIERGE_DATA                                               |                                                  |
| 0x29 | COMMON_RET_CONCIERGE_DATA                                               |                                                  |
| 0x2e | COMMON_SET_LINK_CONTROL                                                 |                                                  |
| 0x2f | COMMON_NTFY_LINK_CONTROL                                                |                                                  |
| 0x34 | UPDT_SET_STATUS                                                         |                                                  |
| 0x35 | UPDT_NTFY_STATUS                                                        |                                                  |
| 0x36 | UPDT_GET_PARAM                                                          |                                                  |
| 0x37 | UPDT_RET_PARAM                                                          |                                                  |
| 0x40 | VPT_GET_CAPABILITY                                                      |                                                  |
| 0x41 | VPT_RET_CAPABILITY                                                      |                                                  |
| 0x42 | VPT_GET_STATUS                                                          |                                                  |
| 0x43 | VPT_RET_STATUS                                                          |                                                  |
| 0x45 | VPT_NTFY_STATUS                                                         |                                                  |
| 0x46 | VPT_GET_PARAM                                                           |                                                  |
| 0x47 | VPT_RET_PARAM                                                           |                                                  |
| 0x48 | VPT_SET_PARAM                                                           |                                                  |
| 0x49 | VPT_NTFY_PARAM                                                          |                                                  |
| 0x50 | EQ_EBB_GET_CAPABILITY                                                   |                                                  |
| 0x51 | EQ_EBB_RET_CAPABILITY                                                   |                                                  |
| 0x52 | EQ_EBB_GET_STATUS                                                       |                                                  |
| 0x53 | EQ_EBB_RET_STATUS                                                       |                                                  |
| 0x55 | EQ_EBB_NTFY_STATUS                                                      |                                                  |
| 0x56 | EQ_EBB_GET_PARAM                                                        |                                                  |
| 0x57 | EQ_EBB_RET_PARAM                                                        |                                                  |
| 0x58 | EQ_EBB_SET_PARAM                                                        |                                                  |
| 0x59 | EQ_EBB_NTFY_PARAM                                                       |                                                  |
| 0x5a | EQ_EBB_GET_EXTENDED_INFO                                                |                                                  |
| 0x5b | EQ_EBB_RET_EXTENDED_INFO                                                |                                                  |
| 0x60 | NC_ASM_GET_CAPABILITY                                                   |                                                  |
| 0x61 | NC_ASM_RET_CAPABILITY                                                   |                                                  |
| 0x62 | NC_ASM_GET_STATUS                                                       |                                                  |
| 0x63 | NC_ASM_RET_STATUS                                                       |                                                  |
| 0x65 | NC_ASM_NTFY_STATUS                                                      |                                                  |
| 0x66 | NC_ASM_GET_PARAM                                                        |                                                  |
| 0x67 | NC_ASM_RET_PARAM                                                        |                                                  |
| 0x68 | NC_ASM_SET_PARAM                                                        |                                                  |
| 0x69 | NC_ASM_NTFY_PARAM                                                       |                                                  |
| 0x70 | SENSE_GET_CAPABILITY                                                    |                                                  |
| 0x71 | SENSE_RET_CAPABILITY                                                    |                                                  |
| 0x74 | SENSE_SET_STATUS                                                        |                                                  |
| 0x80 | OPT_GET_CAPABILITY                                                      |                                                  |
| 0x81 | OPT_RET_CAPABILITY                                                      |                                                  |
| 0x82 | OPT_GET_STATUS                                                          |                                                  |
| 0x83 | OPT_RET_STATUS                                                          |                                                  |
| 0x84 | OPT_SET_STATUS                                                          |                                                  |
| 0x85 | OPT_NTFY_STATUS                                                         |                                                  |
| 0x86 | OPT_GET_PARAM                                                           |                                                  |
| 0x87 | OPT_RET_PARAM                                                           |                                                  |
| 0x89 | OPT_NTFY_PARAM                                                          |                                                  |
| 0x90 | ALERT_GET_CAPABILITY                                                    |                                                  |
| 0x91 | ALERT_RET_CAPABILITY                                                    |                                                  |
| 0x94 | ALERT_SET_STATUS                                                        |                                                  |
| 0x98 | ALERT_SET_PARAM                                                         |                                                  |
| 0x99 | ALERT_NTFY_PARAM                                                        |                                                  |
| 0xa0 | PLAY_GET_CAPABILITY                                                     |                                                  |
| 0xa1 | PLAY_RET_CAPABILITY                                                     |                                                  |
| 0xa2 | PLAY_GET_STATUS                                                         |                                                  |
| 0xa3 | PLAY_RET_STATUS                                                         |                                                  |
| 0xa4 | PLAY_SET_STATUS                                                         |                                                  |
| 0xa5 | PLAY_NTFY_STATUS                                                        |                                                  |
| 0xa6 | PLAY_GET_PARAM                                                          |                                                  |
| 0xa7 | PLAY_RET_PARAM                                                          |                                                  |
| 0xa8 | PLAY_SET_PARAM                                                          |                                                  |
| 0xa9 | PLAY_NTFY_PARAM                                                         |                                                  |
| 0xb0 | SPORTS_GET_CAPABILITY                                                   |                                                  |
| 0xb1 | SPORTS_RET_CAPABILITY                                                   |                                                  |
| 0xb2 | SPORTS_GET_STATUS                                                       |                                                  |
| 0xb3 | SPORTS_RET_STATUS                                                       |                                                  |
| 0xb5 | SPORTS_NTFY_STATUS                                                      |                                                  |
| 0xb6 | SPORTS_GET_PARAM                                                        |                                                  |
| 0xb7 | SPORTS_RET_PARAM                                                        |                                                  |
| 0xb8 | SPORTS_SET_PARAM                                                        |                                                  |
| 0xb9 | SPORTS_NTFY_PARAM                                                       |                                                  |
| 0xba | SPORTS_GET_EXTENDED_PARAM                                               |                                                  |
| 0xbb | SPORTS_RET_EXTENDED_PARAM                                               |                                                  |
| 0xbc | SPORTS_SET_EXTENDED_PARAM                                               |                                                  |
| 0xbd | SPORTS_NTFY_EXTENDED_PARAM                                              |                                                  |
| 0xc4 | LOG_SET_STATUS                                                          |                                                  |
| 0xc9 | LOG_NTFY_PARAM                                                          |                                                  |
| 0xd0 | GENERAL_SETTING_GET_CAPABILITY                                          |                                                  |
| 0xd1 | GENERAL_SETTING_RET_CAPABILITY                                          |                                                  |
| 0xd2 | GENERAL_SETTING_GET_STATUS                                              |                                                  |
| 0xd3 | GENERAL_SETTING_RET_STATUS                                              |                                                  |
| 0xd5 | GENERAL_SETTING_NTFY_STATUS                                             |                                                  |
| 0xd6 | GENERAL_SETTING_GET_PARAM                                               |                                                  |
| 0xd7 | GENERAL_SETTING_RET_PARAM                                               |                                                  |
| 0xd8 | GENERAL_SETTING_SET_PARAM                                               |                                                  |
| 0xd9 | GENERAL_SETTING_NTNY_PARAM                                              |                                                  |
| 0xe0 | AUDIO_GET_CAPABILITY                                                    |                                                  |
| 0xe1 | AUDIO_RET_CAPABILITY                                                    |                                                  |
| 0xe2 | AUDIO_GET_STATUS                                                        |                                                  |
| 0xe3 | AUDIO_RET_STATUS                                                        |                                                  |
| 0xe5 | AUDIO_NTFY_STATUS                                                       |                                                  |
| 0xe6 | AUDIO_GET_PARAM                                                         |                                                  |
| 0xe7 | AUDIO_RET_PARAM                                                         |                                                  |
| 0xe8 | AUDIO_SET_PARAM                                                         |                                                  |
| 0xe9 | AUDIO_NTFY_PARAM                                                        |                                                  |
| 0xf0 | SYSTEM_GET_CAPABILITY                                                   |                                                  |
| 0xf1 | SYSTEM_RET_CAPABILITY                                                   |                                                  |
| 0xf2 | SYSTEM_GET_STATUS                                                       |                                                  |
| 0xf3 | SYSTEM_RET_STATUS                                                       |                                                  |
| 0xf5 | SYSTEM_NTFY_STATUS                                                      |                                                  |
| 0xf6 | SYSTEM_GET_PARAM                                                        |                                                  |
| 0xf7 | SYSTEM_RET_PARAM                                                        |                                                  |
| 0xf8 | SYSTEM_SET_PARAM                                                        |                                                  |
| 0xf9 | SYSTEM_NTFY_PARAM                                                       |                                                  |
| 0xfa | SYSTEM_GET_EXTENDED_PARAM                                               |                                                  |
| 0xfb | SYSTEM_RET_EXTENDED_PARAM                                               |                                                  |
| 0xfc | SYSTEM_SET_EXTENDED_PARAM                                               |                                                  |
| 0xfd | SYSTEM_NTFY_EXTENDED_PARAM                                              |                                                  |
| 0xff | TEST_COMMAND                                                            |                                                  |
