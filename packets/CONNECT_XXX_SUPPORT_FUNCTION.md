# CONNECT_(GET/RET)_SUPPORT_FUNCTION

<br/>

## CONNECT_GET_SUPPORT_FUNCTION

| offset | length | meaning            |
|:------:|:------:|:-------------------|
| `0x00` | 1 byte | fixed value `0x00` |

<br/>

## CONNECT_RET_SUPPORT_FUNCTION

| offset | length | meaning                                        |
|:------:|:------:|:-----------------------------------------------|
| `0x00` | 1 byte | fixed value `0x00`                             |
| `0x01` | 1 byte | [FunctionType](#enum-functiontype) count (`N`) |
| `0x02` |   N    | list of [FunctionType](#enum-functiontype)s    |

<br/>

### enum FunctionType

|  byte  | meaning                                 |
|:------:|:----------------------------------------|
| `0x11` | BATTERY_LEVEL                           |
| `0x12` | UPSCALING_INDICATOR                     |
| `0x13` | CODEC_INDICATOR                         |
| `0x14` | BLE_SETUP                               |
| `0x15` | LEFT_RIGHT_BATTERY_LEVEL                |
| `0x17` | LEFT_RIGHT_CONNECTION_STATUS            |
| `0x18` | CRADLE_BATTERY_LEVEL                    |
| `0x21` | POWER_OFF                               |
| `0x22` | CONCIERGE_DATA                          |
| `0x23` | TANDEM_KEEP_ALIVE                       |
| `0x30` | FW_UPDATE                               |
| `0x38` | PAIRING_DEVICE_MANAGEMENT_CLASSIC_BT    |
| `0x39` | VOICE_GUIDANCE                          |
| `0x41` | VPT                                     |
| `0x42` | SOUND_POSITION                          |
| `0x51` | PRESET_EQ                               |
| `0x52` | EBB                                     |
| `0x53` | PRESET_EQ_NONCUSTOMIZABLE               |
| `0x61` | NOISE_CANCELLING                        |
| `0x62` | NOISE_CANCELLING_AND_AMBIENT_SOUND_MODE |
| `0x63` | AMBIENT_SOUND_MODE                      |
| `0x71` | AUTO_NC_ASM                             |
| `0x81` | NC_OPTIMIZER                            |
| `0x92` | VIBRATOR_ALERT_NOTIFICATION             |
| `0xa1` | PLAYBACK_CONTROLLER                     |
| `0xb1` | TRAINING_MODE                           |
| `0xc1` | ACTION_LOG_NOTIFIER                     |
| `0xd1` | GENERAL_SETTING_1                       |
| `0xd2` | GENERAL_SETTING_2                       |
| `0xd3` | GENERAL_SETTING_3                       |
| `0xe1` | CONNECTION_MODE                         |
| `0xe2` | UPSCALING                               |
| `0xf1` | VIBRATOR                                |
| `0xf2` | POWER_SAVING_MODE                       |
| `0xf3` | CONTROL_BY_WEARING                      |
| `0xf5` | SMART_TALKING_MODE                      |
| `0xf4` | AUTO_POWER_OFF                          |
| `0xf6` | ASSIGNABLE_SETTINGS                     |

