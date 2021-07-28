# COMMON_(GET/RET)_BATTERY_LEVEL

<br/>

## COMMON_GET_BATTERY_LEVEL

| offset | length | meaning                                   |
|:------:|:------:|:------------------------------------------|
| `0x00` | 1 byte | [InquiredType](#enum-batteryinquiredtype) |

A `COMMON_GET_BATTERY_LEVEL` packets should only be sent if the devices supports a particular BatteryInquiredType, this should be checked beforehand using `CONNECT_GET_SUPPORT_FUNCTION`.

<br/>

## COMMON_RET_BATTERY_LEVEL

| offset | length | meaning                                   |
|:------:|:------:|:------------------------------------------|
| `0x00` | 1 byte | [InquiredType](#enum-batteryinquiredtype) |
| `0x01` |   -    | Depends on __BatteryInquiredType__        |

## COMMON_NTFY_BATTERY_LEVEL

| offset | length | meaning                                   |
|:------:|:------:|:------------------------------------------|
| `0x00` | 1 byte | [InquiredType](#enum-batteryinquiredtype) |
| `0x01` |   -    | Depends on __BatteryInquiredType__        |

<br/>

### enum BatteryInquiredType

|  byte  | meaning                                              |
|:------:|:-----------------------------------------------------|
| `0x00` | [BATTERY](#payload-if-battery)                       |
| `0x02` | [LEFT_RIGHT_BATTERY](#payload-if-left_right_battery) |
| `0x03` | [CRADLE_BATTERY](#payload-if-cradle_battery)         |

<br/>

### Payload if __BATTERY__

| offset | length | meaning                               |
|:------:|:------:|:--------------------------------------|
| `0x00` | 1 byte | __BatteryInquiredType__ = __BATTERY__ |
| `0x01` | 1 byte | Battery level (%)                     |
| `0x02` | 1 byte | Is charging (0/1)                     |

<br/>

### Payload if __LEFT_RIGHT_BATTERY__

| offset | length | meaning                                          |
|:------:|:------:|:-------------------------------------------------|
| `0x00` | 1 byte | __BatteryInquiredType__ = __LEFT_RIGHT_BATTERY__ |
| `0x01` | 1 byte | Left battery level (%)                           |
| `0x02` | 1 byte | Left is charging (0/1)                           |
| `0x03` | 1 byte | Right battery level (%)                          |
| `0x04` | 1 byte | Right is charging (0/1)                          |

<br/>

### Payload if __CRADLE_BATTERY__

| offset | length | meaning                                      |
|:------:|:------:|:---------------------------------------------|
| `0x00` | 1 byte | __BatteryInquiredType__ = __CRADLE_BATTERY__ |
| `0x01` | 1 byte | Battery level (%)                            |
| `0x02` | 1 byte | Is charging (0/1)                            |
