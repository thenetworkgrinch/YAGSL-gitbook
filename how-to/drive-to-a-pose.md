---
description: Command the robot to a field-relative Pose2d without PathPlanner.
---

# How to drive to a pose

`SwerveDrive.driveToPose(Pose2d)` drives straight to a field-relative pose using two PID
controllers on `SwerveDriveConfig` — no path planning involved, just "go here." It's a good fit for
short, single-target moves: aligning to a reef face, a coral station, or a fixed AprilTag-relative
setpoint. For multi-point autonomous routines with obstacle-aware paths, see
[How to set up PathPlanner](setup-pathplanner.md) instead.

{% hint style="warning" %}
`driveToPose` is **not compatible with AdvantageKit** replay (per its javadoc) — it reads gyro/pose
state live inside the command rather than through a logged input.
{% endhint %}

## 1. Give the drive translation and rotation controllers

`SwerveParser` doesn't set these for you — there's no JSON field for them, since they're a
robot-code concern, not a per-robot hardware description. Set them on the `SwerveDriveConfig`
before calling `createSwerveDrive(...)`:

```java
var cfg = new SwerveDriveConfig()
    .withStartingPose(new Pose2d(3, 3, Rotation2d.kZero))
    .withSubsystem(this)
    .withTelemetry(TelemetryVerbosity.HIGH)
    .withTranslationController(new PIDController(1.0, 0, 0)) // input: meters of position error
    .withRotationController(new PIDController(1.0, 0, 0));   // input: radians of heading error

drive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve/base"))
    .createSwerveDrive(cfg);
```

{% hint style="danger" %}
If you call `driveToPose()` without setting both controllers first, expect a
`NoSuchElementException` — `SwerveDriveConfig` throws rather than silently picking a default, since
an untuned default would just as likely fight the robot as help it.
{% endhint %}

These are the same controllers `SwerveInputStream`'s heading-hold/heading-lock control uses (see
[Heading Correction](../explanation/module-behaviors.md#heading-correction-heading-snap-control)) —
if you've already tuned heading-hold, you have a reasonable starting point for `driveToPose` too,
though the two use cases (holding a heading vs. closing a rotation error to a specific target) can
want different aggressiveness.

## 2. Wrap it in a command that knows when to stop

`driveToPose` never finishes on its own — it's a `drive(...)` loop that keeps correcting toward the
target pose forever, the same way your teleop default command never finishes. Race it against a
tolerance check (or a timeout) so it actually ends:

```java
public Command driveToPose(Pose2d pose) {
  return drive.driveToPose(pose)
      .until(() -> drive.getDistanceFromPose(pose).in(Inches) < 1
                && Math.abs(drive.getAngleDifferenceFromPose(pose).in(Degrees)) < 2);
}
```

`SwerveDrive.getDistanceFromPose(Pose2d)` and `getAngleDifferenceFromPose(Pose2d)` give you the
current translation/rotation error against any pose, so the tolerance check above works for any
target, not just the one you started the command with.

If you'd rather hold at the pose indefinitely (e.g. as a default command while lined up on a game
piece), skip `.until(...)` and just interrupt it with whatever takes over next — that's exactly
what a bare `drive.driveToPose(pose)` binding does.

## 3. Bind it

```java
driverXbox.a().onTrue(swerve.driveToPose(new Pose2d(3, 3, Rotation2d.fromDegrees(30))));
```

Pass a fixed pose for a known field feature, or a pose computed on the fly (from vision, or the
nearest of a set of candidate poses) — `driveToPose` doesn't care where the `Pose2d` came from.

{% hint style="info" %}
The pose is field-relative with the blue-alliance wall as the origin, `0°` facing the red alliance
wall — the same convention `SwerveDrive.getPose()`/odometry already use. If you're computing poses
per-alliance, flip them the same way `SwerveInputStream.withAllianceRelativeControl()` does rather
than hand-rolling it.
{% endhint %}
