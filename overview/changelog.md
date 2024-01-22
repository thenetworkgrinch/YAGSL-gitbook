# Changelog

## Pull Requests are always reviewed!

I highly encourage anyone who wants to help make YAGSL better to create pull requests with any modifications you have made that increases your quality of life.&#x20;

{% embed url="https://github.com/BroncBotz3481/YAGSL-Example/pulls?q=is%3Apr+is%3Aclosed" %}

## Contributing

YAGSL development is done on the YAGSL-Example repository's `dev` branch here at `src/main/java/swervelib`.

{% embed url="https://github.com/BroncBotz3481/YAGSL-Example/tree/dev" %}
YAGSL-Example dev branch
{% endembed %}

All PR's should be based off of and merged into here. YAGSL is propogated often to the other repositories.

## 2024.4.6.1

* [ ] Fixed `SwerveDrive.resetOdometry` and utilize the pose estimation instead. (PR [#142](https://github.com/BroncBotz3481/YAGSL-Example/pull/142) by [@MarshallTappen](https://github.com/MarshallTappen) and @nstrike [commit](https://github.com/BroncBotz3481/YAGSL-Example/commit/039e5c2867690cfdd5ebd0c1e84eefc6b165adee))
* [ ] Functionalize IMU inversion (PR [#140](https://github.com/BroncBotz3481/YAGSL-Example/pull/140) by [@TechnologyMan00](https://github.com/Technologyman00) and @nstrike)
* [ ] Added `SwerveDrive.getOdometryHeading()`
* [ ] Fixed `SwerveDrive.addVisionMeasurement` with vision standard deviations.
* [ ] Added IMU readings to SmartDashboard via `Raw IMU Yaw` (gyro with invert applied) and `Adjusted IMU Yaw` (pose estimation rotation).
* [ ] Changed `navx_mxp` to `navx_mxp_serial` to notate that it's serial over MXP.
* [ ] Added warning when using `navx_mxp_serial` or `navx_usb`.
* [ ] Added back wheel speed desaturation.

## 2024.4.6

* [ ] Simplified `SwerveMath.calculateDegreesPerRotation` and `SwerveMath.calculateMetersPerRotation` to exclude encoder resolution and add a default.
* [ ] Changed `Module[...] Raw Motor Encoder` to `Module[...] Raw Angle Encoder`.
* [ ] Added `Module[...] Raw Drive Encoder`

## 2024.4.5

* [ ] Fix for TalonFX Angle motor control (by [@bhall-ctre](https://github.com/bhall-ctre), and @Wackyvert 2225 Mentor) .
  1. TalonFX's needed to use conversion factor as gear ratio rather than gear ratio + unit conversion.&#x20;
  2. Conversion factor needed to be inverted.
  3. setPosition update to reflect current position.
* [ ] Changed `ma3` encoders to be read via analog input. (Discovered by [@CoZm0](https://github.com/CoZ-m0))
* [ ] Support 3 wheel swerve module setups with PathPlanner helper function. (PR #139 by [@TechnologyMan00](https://github.com/Technologyman00))
* [ ] Added `SwerveModule.getAbsoluteEncoder()` `SwerveDrive.getMaximumVelocity()` and `SwerveDrive.getMaximumAngularVelocity()`.
* [ ] Reccommend Tuner X when a compatible Tuner X config is used.
* [ ] Added ability to change heading correction deadband.

## 2024.4.2

* [ ] Added feedforward to SparkMAX's.
* [ ] Added Network Alerts (made by[ @TheGamer1002](https://github.com/TheGamer1002) PR [#136](https://github.com/BroncBotz3481/YAGSL-Example/pull/136))
* [ ] Added feedforward to TalonFX's
* [ ] Fixed TalonFX conversion factor (made by [@jkbo6](https://github.com/jbko6) PR [#128](https://github.com/BroncBotz3481/YAGSL-Example/pull/128))
* [ ] CanAndCoder tweaks (made by [@guineahawk](https://github.com/guineawheek) PR [#134](https://github.com/BroncBotz3481/YAGSL-Example/pull/134))
* [ ] Added missing SparkMAX status frame's (made by [@RoboPenguin7](https://github.com/RoboPenguin7) PR [#130](https://github.com/BroncBotz3481/YAGSL-Example/pull/130))
* [ ] Heading Correction update to make it less sensitive (made by [@balien-12](https://github.com/balien-12) PR [#132](https://github.com/BroncBotz3481/YAGSL-Example/pull/132))
* [ ] Fixed cosine compensator error (made by [@jkbo6](https://github.com/jbko6) PR [#129](https://github.com/BroncBotz3481/YAGSL-Example/pull/129))
* [ ] Updated PathPlanner path based off of alliance (made by [@MarshalTappen](https://github.com/MarshallTappen) PR [#122](https://github.com/BroncBotz3481/YAGSL-Example/pull/122))
* [ ] Fixed Rotation being off, update getDriveBaseMeters (made by [@TechnologyMan00](https://github.com/Technologyman00))
