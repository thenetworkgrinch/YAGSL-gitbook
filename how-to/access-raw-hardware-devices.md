---
description: Get the actual vendor motor controller/encoder/gyro objects SwerveParser built, for configuration that isn't exposed through SmartMotorController.
---

# How to access raw hardware devices

{% hint style="info" %}
Most robot code never needs this page. `SwerveDrive`, `SwerveModule`, and YAMS's
`SmartMotorController` cover driving, telemetry, and the vast majority of per-motor
configuration. Reach for `createSwerveDriveDevices` when you need to call a vendor-specific
method (a CTRE `TalonFXConfigurator` option, a REV `SparkMax` signal, a `CANcoder`-specific
setting) that `SmartMotorController` doesn't expose, or when you need real hardware, not a
wrapper, for something like MegaTag2 vision fusion (see the example below).
{% endhint %}

`SwerveParser.createSwerveDrive(config)` hands you back a fully-built `SwerveDrive`. Everything
underneath it (drive/azimuth motor controllers, absolute encoders, the gyro) is wrapped and
effectively private. `SwerveParser.createSwerveDriveDevices(config)` builds the exact same
hardware, but also gives you the raw vendor objects it created along the way, alongside the
`SwerveDrive` itself.

## 1. Call `createSwerveDriveDevices` instead of `createSwerveDrive`

```java title="SwerveDriveSubsystem.java"
import swervelib.parser.SwerveParser;
import swervelib.parser.SwerveParser.SwerveDriveDevices;

SwerveParser.parse(new File(Filesystem.getDeployDirectory(), "swerve/base"));
SwerveDriveDevices devices = SwerveParser.createSwerveDriveDevices(cfg);

drive = devices.swerveDrive();
```

{% hint style="danger" %}
Call **either** `createSwerveDrive(...)` **or** `createSwerveDriveDevices(...)` for a given
parsed directory, not both. Each call builds the hardware from scratch, so calling both would
construct every motor controller, encoder, and gyro twice (two CAN objects fighting over the
same ID).
{% endhint %}

## 2. Read the devices back out

`SwerveDriveDevices` is a record with three components:

| Component     | Type                     | Notes                                                                                        |
| -------------- | ------------------------ | ---------------------------------------------------------------------------------------------- |
| `swerveDrive`  | `SwerveDrive`            | The same object `createSwerveDrive(...)` would have returned. Use it exactly as normal.        |
| `gyro`         | `Object`                 | The raw gyro device (e.g. a CTRE `Pigeon2`), or `null` if `gyro.type` is `"custom"`. See below. |
| `modules`      | `SwerveModuleDevices[]`  | One entry per module, in the same order as the `modules` array in `swervedrive.json`.          |

Each `SwerveModuleDevices` is in turn:

| Component         | Type     | Notes                                                                                                                                     |
| ------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `drive`             | `Object` | Raw drive motor controller (e.g. a REV `SparkMax`/`SparkFlex`, or a CTRE `TalonFX`/`TalonFXS`).                                          |
| `azimuth`           | `Object` | Raw azimuth/angle motor controller.                                                                                                     |
| `absoluteEncoder`   | `Object` | Raw absolute encoder device: a separate device (e.g. `CANcoder`, `AnalogEncoder`, `DutyCycleEncoder`), or the same object as `azimuth` if the encoder is attached to the azimuth controller. |

Cast each `Object` to whatever vendor type you know it to be from your `swervedrive.json`/module
JSON `type` strings (see [Device Type Strings](../reference/json-schema/device-types.md)):

```java
import com.ctre.phoenix6.hardware.TalonFX;
import com.ctre.phoenix6.hardware.CANcoder;

SwerveModuleDevices frontLeft = devices.modules()[0];

TalonFX driveMotor = (TalonFX) frontLeft.drive();
CANcoder absoluteEncoder = (CANcoder) frontLeft.absoluteEncoder();

driveMotor.getConfigurator().apply(myCustomTalonConfig);
```

{% hint style="warning" %}
Nothing enforces that `modules[i]` is the type you expect. Mixed-vendor swerve drives (a CTRE
drive motor with a REV angle motor, say) are supported by YAGSL, so a blind cast across every
module in a loop can throw `ClassCastException` if the vendors aren't actually uniform. Cast
per-module against what that module's JSON actually specifies.
{% endhint %}

## 3. Example: full 3-axis gyro data for MegaTag2

`SwerveDrive.getGyroAngle()` only ever gives you yaw, which is genuinely all MegaTag2 requires
(see [How to use a custom gyro](use-a-custom-gyro.md#optional-angular-velocity-for-skew-correction)
for the same point applied to skew correction). But if your gyro is a full IMU and you'd like to
feed a vision coprocessor real roll and pitch too, you need the raw device, not the wrapped one:

```java title="SwerveDriveSubsystem.java"
import com.ctre.phoenix6.StatusSignal;
import com.ctre.phoenix6.hardware.Pigeon2;
import edu.wpi.first.units.measure.AngularVelocity;

private Pigeon2 gyro;
private StatusSignal<AngularVelocity> gyroX;
private StatusSignal<AngularVelocity> gyroY;
private StatusSignal<AngularVelocity> gyroZ;

public SwerveDriveSubsystem() {
  // ... parse and build cfg as usual ...
  SwerveDriveDevices devices = SwerveParser.createSwerveDriveDevices(cfg);
  drive = devices.swerveDrive();

  gyro = (Pigeon2) devices.gyro();
  gyroX = gyro.getAngularVelocityXDevice();
  gyroY = gyro.getAngularVelocityYDevice();
  gyroZ = gyro.getAngularVelocityZDevice();
}

public Rotation3d getGyroRotation3d() {
  return gyro.getRotation3d(); // full roll/pitch/yaw, not just the yaw SwerveDrive tracks
}
```

Then hand that to your vision coprocessor's orientation input the same way you would the
yaw-only version, for example YALL's `LimelightSettings.withRobotOrientation(...)`. Whether the
extra roll/pitch actually improves your pose estimate depends on your camera mount and how noisy
that IMU data is; it isn't automatically better than yaw-only, just more complete.

{% hint style="info" %}
This pattern is vendor-specific: `getRotation3d()` and per-axis `StatusSignal`s are CTRE Pigeon2
methods. A different gyro's raw device exposes full orientation and angular velocity differently,
or not at all, in which case yaw-only (from `SwerveDrive.getGyroAngle()`) is what you have, and
that's fine, it's all MegaTag2 needs anyway.
{% endhint %}

## 4. Handle a `custom` gyro

If `swervedrive.json`'s `gyro.type` is `"custom"`, the parser never builds a gyro device at all,
see [How to use a custom gyro](use-a-custom-gyro.md), so `devices.gyro()` is `null`. Your own
gyro object (the one you constructed to call `SwerveDriveConfig.withGyro(...)`) is already in
scope wherever you built it; there's nothing to retrieve here.

## Why `Object`?

`SwerveModuleDevices`/`SwerveDriveDevices` are deliberately vendor-agnostic. YAGSL supports
mixing CTRE, REV, ThriftyBot, AndyMark, and Redux hardware across modules (see
[Supported Hardware](../reference/hardware/README.md)), so there's no single common supertype to
return instead of `Object`. The cast is the price of that flexibility.
