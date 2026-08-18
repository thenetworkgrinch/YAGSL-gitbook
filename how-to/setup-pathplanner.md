---
description: Register a YAGSL-built SwerveDrive with PathPlanner's AutoBuilder.
---

# How to set up PathPlanner

[PathPlanner](https://pathplanner.dev) is a separate tool from YAGSL — it plans and follows
multi-point paths (with obstacle-aware routing, event markers, and an on-field GUI editor) rather
than the single point-to-point moves in [driveToPose](drive-to-a-pose.md). This page wires a
YAGSL-built `SwerveDrive` into it. It assumes you already have a driving robot from the
[Tutorial](../tutorial/README.md).

{% hint style="info" %}
This page covers the YAGSL/YAMS side of the wiring. For building paths/autos themselves, installing
the standalone PathPlanner GUI app, and general PathPlanner concepts, see
[PathPlanner's own documentation](https://pathplanner.dev/home.html).
{% endhint %}

## 1. Install PathplannerLib

Open the **WPILib Vendor Dependencies** panel in VS Code (same place you installed YAGSL — see
[Install YAGSL](../tutorial/02-install-yagsl.md)) and install **PathplannerLib** from the catalog.

## 2. Export your robot config from the PathPlanner GUI

In the [PathPlanner GUI app](https://github.com/mjansen4857/pathplanner/releases), fill in your
robot's mass, MOI, module offsets, wheel radius, and drive motor specs under its Robot Config tab,
then export. This writes `deploy/pathplanner/settings.json`, which
`RobotConfig.fromGUISettings()` reads at runtime — you don't hand-write this file or duplicate its
values in Java.

{% hint style="warning" %}
This is a **second, separate** description of your robot's physical properties from YAGSL's own
`modules/physicalproperties.json` — gear ratio and wheel diameter in particular exist in both
places. If you change one (a wheel swap, a gearing change), update the other too, or PathPlanner's
paths and YAGSL's own odometry will disagree about how far the robot actually travels. This is a
common, JSON-invisible cause of drift — see the
[PathPlanner section of Swerve Drive Drift](../reference/swerve-drift-causes.md#pathplanner).
{% endhint %}

## 3. Register `AutoBuilder` once, right after building the drive

Do this once, in your swerve subsystem's constructor, right after `SwerveParser.createSwerveDrive(...)`
returns. After this call, `PathPlannerAuto` commands and GUI-defined paths just work — no extra
wiring per auto.

```java
import com.pathplanner.lib.auto.AutoBuilder;
import com.pathplanner.lib.config.PIDConstants;
import com.pathplanner.lib.config.RobotConfig;
import com.pathplanner.lib.controllers.PPHolonomicDriveController;
import edu.wpi.first.wpilibj.DriverStation;
import java.io.IOException;
import org.json.simple.parser.ParseException;

public class SwerveDriveSubsystem extends SubsystemBase {
  private SwerveDrive drive;

  public SwerveDriveSubsystem() {
    var cfg = new SwerveDriveConfig()
        .withStartingPose(new Pose2d(3, 3, Rotation2d.kZero))
        .withSubsystem(this)
        .withTelemetry(TelemetryVerbosity.HIGH);

    drive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve/base"))
        .createSwerveDrive(cfg);

    try {
      setupPathPlanner();
    } catch (IOException | ParseException e) {
      throw new RuntimeException(
          "PathPlanner setup failed -- check deploy/pathplanner/settings.json exists", e);
    }
  }

  private void setupPathPlanner() throws IOException, ParseException {
    AutoBuilder.configure(
        drive::getPose,                  // robot pose supplier
        drive::resetOdometry,             // called if an auto defines a starting pose
        drive::getRobotRelativeSpeed,     // ChassisSpeeds supplier -- MUST be robot-relative
        (speedsRobotRelative, moduleFeedForwards) ->
            drive.setRobotRelativeChassisSpeeds(speedsRobotRelative),
        new PPHolonomicDriveController(
            new PIDConstants(5.0, 0.0, 0.0),  // translation PID
            new PIDConstants(5.0, 0.0, 0.0)   // rotation PID
        ),
        RobotConfig.fromGUISettings(),    // reads deploy/pathplanner/settings.json
        () -> {
          // Field origin is always the blue alliance wall -- flip paths when on red.
          var alliance = DriverStation.getAlliance();
          return alliance.filter(a -> a == DriverStation.Alliance.Red).isPresent();
        },
        this                              // subsystem requirement for the generated commands
    );
  }
}
```

{% hint style="warning" %}
The `PPHolonomicDriveController` PID constants above are **independent** of
`SwerveDriveConfig.withTranslationController()`/`withRotationController()` from
[driveToPose](drive-to-a-pose.md) — PathPlanner's `AutoBuilder` uses its own controller and never
touches YAGSL's. Tune them separately; there's no reason they need matching gains.
{% endhint %}

## 4. Run an auto

```java
public Command getAutonomousCommand() {
  return new PathPlannerAuto("New Auto");
}
```

Build `"New Auto"` in the PathPlanner GUI, deploy, and bind `getAutonomousCommand()` the same way
you'd bind any other autonomous command (`RobotContainer`'s autonomous command supplier, or
`Robot.autonomousInit()`). Event markers placed in the GUI fire `EventTrigger`s in code:

```java
new EventTrigger("EventMarker").whileTrue(Commands.print("Something"));
```

## Troubleshooting

If a PathPlanner auto drives the wrong distance, curves when it should be straight, or ends up in
the wrong spot, see the [PathPlanner checklist in Swerve Drive Drift](../reference/swerve-drift-causes.md#pathplanner)
before re-tuning anything — most of these trace back to the config mismatch in step 2, or PID gains
that were never tuned past their defaults.
