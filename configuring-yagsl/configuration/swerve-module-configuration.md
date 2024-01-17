# Swerve Module Configuration

## Swerve Module Configuration (`module/x.json`)

The swerve module configuration configures unique properties of each swerve module. It maps 1:1 with [`ModuleJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/ModuleJson.html) which is used to create [`SwerveModuleConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html). This configuration file interacts directly with swerve kinematics.

{% hint style="warning" %}
**IF** your angle motors spin out of control then you may need to invert them in `inverted`
{% endhint %}

## Fields

| Name                      | Units                                                                                                                                                                                                                                            | Required | Description                                                                                                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `drive`                   | [Motor Controller](../../devices/motor-controllers/#motor-controller-configuration)                                                                                                                                                              | Y        | Drive motor device configuration.                                                                                                                                                                 |
| `angle`                   | [Motor Controller](../../devices/motor-controllers/#motor-controller-configuration)                                                                                                                                                              | Y        | Angle motor device configuration.                                                                                                                                                                 |
| `encoder`                 | Absolute Encoder                                                                                                                                                                                                                                 | Y        | Absolute encoder device configuration.                                                                                                                                                            |
| `inverted`                | [MotorConfig](swerve-module-configuration.md#motorconfig)                                                                                                                                                                                        | Y        | Inversion state of each motor as a boolean.                                                                                                                                                       |
| `absoluteEncoderOffset`   | Degrees                                                                                                                                                                                                                                          | Y        | Absolute encoder offset from 0 in degrees. May need to be a negative number.                                                                                                                      |
| `absoluteEncoderInverted` | Bool                                                                                                                                                                                                                                             | N        | Inversion state of the Absolute Encoder.                                                                                                                                                          |
| `location`                | [Location](swerve-module-configuration.md#location)                                                                                                                                                                                              | Y        | The location of the swerve module from the center of the robot in inches. +x is torwards the robot front, and +y is torwards robot left.                                                          |
| `conversionFactor`        | <p><a href="swerve-module-configuration.md#motorconfig">MotorConfig</a><br>Overrides the value in <a href="physical-properties-configuration.md#fields"><code>physicalproperties.json</code></a> if present and not equal to <code>0</code>.</p> | N        | _OVERRIDE_ Conversion factor applied to the motor controller for the onboard PID, used to override this setting in [`swervedrive.json`](https://github.com/BroncBotz3481/YAGSL/wiki/Swerve-Drive) |

#### MotorConfig

| Name    | Units | Required | Description        |
| ------- | ----- | -------- | ------------------ |
| `drive` | Value | Y        | Drive motor value. |
| `angle` | Value | Y        | Angle motor value. |

#### Location

| Name    | Units | Required | Description                                              |
| ------- | ----- | -------- | -------------------------------------------------------- |
| `front` | Value | Y        | Inches from robot center to module in forward direction. |
| `left`  | Value | Y        | Inches from robot center to module in left direction.    |

