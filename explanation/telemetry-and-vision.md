---
description: Telemetry verbosity, simulation, and fusing vision pose estimates
---

# Telemetry, Simulation & Vision

## Telemetry

YAMS pushes swerve drive and module data to NetworkTables under `SmartDashboard/swerve`, with per-module data under `SmartDashboard/swerve/modules/<name>/`. This is the data most dashboards (Shuffleboard, Elastic, AdvantageScope, FRC Web Components) read to render swerve widgets — see the [AdvantageScope](../how-to/set-up-advantagescope.md) and [FRC Web Components](../how-to/set-up-frc-web-components.md) how-to guides.

How much gets published is controlled by `yams.motorcontrollers.SmartMotorControllerConfig.TelemetryVerbosity`:

```java
public enum TelemetryVerbosity {
  LOW,   // minimal telemetry
  MID,   // moderate telemetry
  HIGH   // full swerve drive + module data
}
```

{% hint style="info" %}
This enum replaces the old `NONE`/`LOW`/`INFO`/`POSE`/`HIGH`/`MACHINE` levels from pre-2026 YAGSL. If you're migrating an older robot project, map your old verbosity choice down to the nearest of `LOW`/`MID`/`HIGH` — see [Schema Changes](../reference/schema-changes.md).
{% endhint %}

```java
var cfg = new SwerveDriveConfig()
    .withTelemetry(TelemetryVerbosity.HIGH);

drive = new SwerveParser(directory).createSwerveDrive(cfg);
```

{% hint style="warning" %}
Higher telemetry verbosity can induce some lag on the robot and slow down loop cycle times — be deliberate about what you choose, especially in competition.
{% endhint %}

### Reading module telemetry while bringing up a robot

Each module publishes its raw absolute encoder and raw angle (relative) encoder readings under `SmartDashboard/swerve/modules/<name>/`. This is the single most useful pair of values while bringing up a new robot:

* If the **Raw Angle Encoder** decreases while the module is rotated counter-clockwise (should be CCW+), the angle motor needs to be inverted in that module's configuration.
* If the **Raw Absolute Encoder** decreases while the wheel is rotated counter-clockwise (should be CCW+), the absolute encoder needs to be inverted for that module.

See [Determine Motor/Encoder Inversion](../how-to/determine-inversion.md) for the full procedure.

## Simulation

YAMS simulates the whole swerve drive using the same vendor simulation models as the real hardware, to varying degrees of fidelity per vendor. All you need to do is call `simIterate()` from your subsystem's `simulationPeriodic()`:

```java
@Override
public void simulationPeriodic()
{
  drive.simIterate();
}
```

{% hint style="info" %}
[Cosine compensation](module-behaviors.md#cosine-compensation) is tuned for real hardware behavior and can look wrong in simulation — this is expected, not a bug in your config.
{% endhint %}

For more on WPILib's simulation framework generally:

{% embed url="https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/introduction.html" %}

## Vision Odometry

YAMS's `SwerveDrive` maintains a `SwerveDrivePoseEstimator` internally, and extends its vision-fusion API directly onto `SwerveDrive` so you don't need to construct or manage your own estimator:

```java
drive.addVisionMeasurement(visionPose, timestampSeconds);

// Or with per-measurement standard deviations:
drive.addVisionMeasurement(visionPose, timestampSeconds, visionStdDevs);

// Or set a persistent default:
drive.setVisionMeasurementStdDevs(visionStdDevs);
```

Feed this from your vision subsystem (PhotonVision, Limelight, etc.) every time a new pose estimate is available. Standard deviations control how much the pose estimator trusts a given vision measurement relative to odometry — tighter (smaller) values pull the estimate toward vision more aggressively; looser (larger) values let odometry dominate. Tune these empirically; see [Tuning Out Drift](../how-to/diagnose-swerve-drift.md) if pose estimates drift or jump unexpectedly.
