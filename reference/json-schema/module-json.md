# Module JSON (per-module file)

One file per swerve module (e.g. `frontleft.json`), referenced by name from `swervedrive.json`'s
`modules` array. Parsed into `swervelib.parser.json.ModuleJson`.

```json
{
  "drive": {
    "type": "sparkmax_neo",
    "id": 1,
    "canbus": ""
  },
  "angle": {
    "type": "sparkmax_neo",
    "id": 2,
    "canbus": ""
  },
  "absoluteEncoder": {
    "type": "revthroughbore_attached",
    "id": 0,
    "channel": 0,
    "canbus": ""
  },
  "inverted": {
    "drive": false,
    "angle": false
  },
  "absoluteEncoderOffset": -45.0,
  "absoluteEncoderInverted": false,
  "location": {
    "front": 12.0,
    "left": 12.0
  }
}
```

| Field                     | Type                              | Required | Description                                                                                                    |
| ------------------------- | --------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `drive`                   | [Device](device-types.md#motor-controllers) | yes | Drive motor controller. See [Device type strings](device-types.md#motor-controllers).               |
| `angle`                   | [Device](device-types.md#motor-controllers) | yes | Angle/steering/azimuth motor controller.                                                            |
| `absoluteEncoder`         | [Device](device-types.md#absolute-encoders) | yes | Module's absolute encoder. See [Device type strings](device-types.md#absolute-encoders).            |
| `gearing`                 | object                             | no       | Per-module override of drive/angle gearing. If omitted, the module uses the defaults from `physicalproperties.json`. Same shape as [physicalproperties.json's `gearing`](physicalproperties-json.md). |
| `inverted.drive`          | boolean                            | yes      | Invert the drive motor.                                                                                        |
| `inverted.angle`          | boolean                            | yes      | Invert the angle motor.                                                                                        |
| `absoluteEncoderOffset`   | number (degrees)                   | yes      | Offset so the wheel reads "forward" when physically pointed forward with the bevel gear to the left. Determined by rotating the wheel to face forward and reading the raw absolute encoder value. |
| `absoluteEncoderInverted` | boolean                            | no (default `false`) | Invert the absolute encoder's reading.                                                            |
| `location.front`          | number (inches)                    | yes      | Distance from the robot's center to the module's center, along the front/back axis.                            |
| `location.left`           | number (inches)                    | yes      | Distance from the robot's center to the module's center, along the left/right axis.                            |

## `drive` / `angle` object

| Field     | Type   | Description                                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------------------------- |
| `type`    | string | Controller+motor type string — see [Device type strings](device-types.md#motor-controllers).    |
| `id`      | number | CAN ID of the motor controller.                                                                  |
| `canbus`  | string | CAN bus the controller is on. `""` for the default bus.                                          |

## `absoluteEncoder` object

| Field     | Type   | Description                                                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------------------------------------------- |
| `type`    | string | Encoder type string — see [Device type strings](device-types.md#absolute-encoders).                          |
| `id`      | number | CAN ID, if the encoder is on CAN. Ignored otherwise.                                                          |
| `channel` | number | AnalogIn channel, if the encoder is analog/duty-cycle. Ignored otherwise.                                     |
| `canbus`  | string | CAN bus, if applicable. `""` for the default bus.                                                             |

{% hint style="info" %}
There is no per-module `useCosineCompensator` toggle anymore — cosine compensation is always applied
by the parser. See [Module Behaviors](../../explanation/module-behaviors.md) for what it does.
{% endhint %}
