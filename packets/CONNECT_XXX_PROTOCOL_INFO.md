# CONNECT_(GET/RET)_PROTOCOL_INFO

<br/>

## CONNECT_GET_PROTOCOL_INFO

Requests protocol info (version). This is the first packet sent by the app.

| offset | length |      meaning       |
|:------:|:------:|:------------------:|
| `0x00` | 1 byte | fixed value `0x00` |

The fixed value (named so in source) is likely reserved for future use.

<br/>

## CONNECT_RET_PROTOCOL_INFO

| offset |   length    |      meaning       |
|:------:|:-----------:|:------------------:|
| `0x00` |   1 byte    | fixed value `0x00` |
| `0x01` | 2 byte (BE) |  protocol version  |

<br/>

## Protocol Version

The protocol version bytes are presumably in a major/minor format.

The following versions: `0x1000`, `0x2000`, `0x3000`, `0x4000`, `0x4010`, `0x5000`, `0x6000`, `0x7000`, and `0x7010`; can be found in the source, in what looks like a compatibility check.

The following checks can also be found, to determine some capabilities, seemingly regarding checks (enough battery/connectivity, etc.) around FW updates.

```
isBleTxPowerSupported: >= 0x2000
isBatteryPowerThresholdSupported: >= 0x2000
isUpdateMethodSupported: >= 0x4000
isBatteryPThresholdForInterruptFWUpdateSupported: >= 0x5000
isUniqueIdForDeviceBindingSupported >= 0x5000
isNotEbbPromotingModel: >= 0x1000
isInstructionGuideSupport: >= 0x5000
canFinishBarometricPressureMeasurementInNoTime: <= 0x5000
```
