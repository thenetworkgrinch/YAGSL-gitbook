# Swerve Drive Configuration

## Swerve Drive (`swervedrive.json`)

The Swerve Drive JSON configuration file configures everything related to the overall Swerve Drive. It maps 1:1 to [`SwerveDriveJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/SwerveDriveJson.html) which creates a [`SwerveDriveConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveDriveConfiguration.html) that is used to create the [`SwerveDrive`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html) object.

## JSON Fields

| Name          | Units                                                            | Required | Description                                                     |
| ------------- | ---------------------------------------------------------------- | -------- | --------------------------------------------------------------- |
| `imu`         | [Gyroscope](../../devices/gyroscope.md#possible-gyroscope-types) | Y        | Robot IMU used to determine heading of the robot.               |
| `invertedIMU` | Boolean                                                          | Y        | Inversion state of the IMU.                                     |
| `modules`     | String array                                                     | Y        | Module JSONs in order clockwise order starting from front left. |

