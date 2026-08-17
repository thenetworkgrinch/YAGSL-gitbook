# swervedrive.json

Top-level drive configuration. Parsed into `swervelib.parser.json.SwerveDriveJson`.

```json
{
  "gyro": {
    "type": "pigeon2_can",
    "id": 0,
    "canbus": ""
  },
  "gyroAxis": "yaw",
  "gyroInvert": false,
  "modules": ["frontleft.json", "frontright.json", "backleft.json", "backright.json"]
}
```

| Field        | Type              | Required | Description                                                                                                    |
| ------------ | ----------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `gyro`       | [Device](device-types.md#gyro) | yes | The robot's gyroscope. See [Device type strings](device-types.md#gyro) for valid `type` values.                |
| `gyroAxis`   | string enum       | no (default `"yaw"`) | Which physical axis of the gyro to use for robot heading: `yaw`, `pitch`, or `roll`.               |
| `gyroInvert` | boolean           | yes      | Invert the gyroscope's heading reading.                                                                        |
| `modules`    | string[]          | yes      | Filenames (relative to the `modules/` directory) of each module's JSON file, minimum 3, listed clockwise from front-left by convention. |

## `gyro` object

| Field     | Type   | Description                                                              |
| --------- | ------ | ------------------------------------------------------------------------- |
| `type`    | string | Gyro type string — see [Device type strings](device-types.md#gyro).      |
| `id`      | number | CAN ID. Set to `0` if the gyro doesn't need one.                          |
| `canbus`  | string | CAN bus name the gyro is on. `""` for the default (rio) bus.               |
