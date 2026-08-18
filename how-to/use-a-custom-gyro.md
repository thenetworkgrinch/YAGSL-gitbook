---
description: Configure a gyro YAGSL's parser doesn't build for you, using a Studica AHRS as the example.
---

# How to use a custom gyro

{% hint style="info" %}
If your gyro is one of the [supported types](../reference/hardware/gyroscopes.md#supported-gyroscope-types)
(Pigeon 2, Canandgyro, NavX3-CAN), you don't need this page — just set `gyro.type` normally in
`swervedrive.json`.
{% endhint %}

Use a `custom` gyro when your hardware isn't one the parser builds for you. The most common case is
the older roboRIO SPI/I2C/USB-serial [Studica AHRS](https://www.studica.com/navx2-micro) (the
classic NavX2) — only the CAN-based NavX3 is supported directly, so an AHRS on the MXP/SPI port
needs this escape hatch. The same approach works for any other IMU, a simulated/composited heading
source, or a heading fused from multiple sensors.

## 1. Set `gyro.type` to `custom`

In `swervedrive.json`, set the gyro's `type` to `custom`. The `id`/`canbus` fields are unused for a
custom gyro (leave them at `0`/`""`), and — this is the part that catches people out — `gyroAxis`
and `gyroInvert` are **also ignored**. The parser sees `custom` and skips gyro configuration
entirely.

```json title="swervedrive.json"
{
  "gyro": { "type": "custom", "id": 0, "canbus": "" },
  "gyroAxis": "yaw",
  "gyroInvert": false,
  "modules": ["frontleft.json", "frontright.json", "backleft.json", "backright.json"]
}
```

## 2. Construct the gyro and supply it yourself

Because the parser never calls `SwerveDriveConfig.withGyro()` for a `custom` gyro, you call it
yourself on the `SwerveDriveConfig` you build **before** handing it to
`SwerveParser.createSwerveDrive(...)`. Everything else — modules, motor controllers, encoders — is
still built normally from the rest of the JSON.

```java title="SwerveDriveSubsystem.java"
import com.studica.frc.AHRS;
import static edu.wpi.first.units.Units.Degrees;

// Onboard MXP SPI port is the common mounting for a roboRIO AHRS.
private final AHRS gyro = new AHRS(AHRS.NavXComType.kMXP_SPI);

public SwerveDriveSubsystem() {
  SwerveDriveConfig cfg = new SwerveDriveConfig()
      .withSubsystem(this)
      .withTelemetry(TelemetryVerbosity.HIGH)
      .withGyro(() -> Degrees.of(gyro.getAngle()));

  drive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve/base"))
      .createSwerveDrive(cfg);
}
```

{% hint style="danger" %}
If you forget to call `withGyro()` before `createSwerveDrive()`, YAMS has no heading supplier to
build the drive with — expect a `NoSuchElementException`/`RuntimeException` at startup, not a
silent fallback.
{% endhint %}

## 3. Handle inversion yourself

Since `gyroInvert` is ignored, do any inversion directly in the `Supplier<Angle>` you pass to
`withGyro()`, then call `withGyroInverted()` yourself to keep it in sync with anything else in
YAMS that reads that flag:

```java
.withGyro(() -> Degrees.of(-gyro.getAngle())) // AHRS reports CW+; YAGSL/YAMS expect CCW+
.withGyroInverted(false)
```

## 4. Verify it

Follow the normal [gyro CCW+ check](determine-inversion.md#2-confirm-the-gyro-reads-ccw) —
`Mechanisms/swerve/gyro` in NetworkTables should still populate and behave exactly like a
parser-built gyro, since from YAMS's perspective a `Supplier<Angle>` is a `Supplier<Angle>`
regardless of where it came from. If it increases while the robot rotates counter-clockwise, you're
done; if not, negate the supplier as in step 3 rather than flipping `gyroInvert` (which, again, is
ignored).

## Optional: angular velocity for skew correction

If you also want gyro-based angular-velocity skew correction (see
[Swerve Drive Drift](../reference/swerve-drift-causes.md)), a `custom` gyro needs its angular
velocity supplied the same way the heading was — the parser doesn't build this for you either,
custom or not:

```java
.withGyroVelocity(() -> DegreesPerSecond.of(-gyro.getRate()))
.withGyroAngularVelocityScaleFactor(1.0)
```

{% hint style="warning" %}
`AHRS.getRate()` and `getAngle()` follow the same sign convention (both CW+ on Studica hardware) —
if you negated one to get CCW+, negate the other too, or your skew correction will fight the
heading it's supposed to be correcting.
{% endhint %}
