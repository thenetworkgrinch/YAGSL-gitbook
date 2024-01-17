# PIDF Properties Configuration

## Swerve Module PID Configuration (`pidfpropreties.json`)

This file configures the PIDF values with integral zone and maximum output of the drive and motor modules for every swerve module. It maps 1:1 to [`PIDFPropertiesJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/PIDFPropertiesJson.html) and is used while initializing the [`SwerveDriveConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveDriveConfiguration.html) object.

## Fields

| Name    | Units                                               | Required | Description                                                           |
| ------- | --------------------------------------------------- | -------- | --------------------------------------------------------------------- |
| `drive` | [PIDF with Integral Zone and Output limit](pidf.md) | Y        | The configuration which will be used for the PIDF on the drive motor. |
| `angle` | [PIDF with Integral Zone and Output limit](pidf.md) | Y        | The configuration which will be used for the PIDF on the angle motor. |

