# 5. Deploy and drive

With hardware verified and offsets captured, wire YAGSL into your robot code. `SwerveParser` reads
your `swerve/base` config directory and builds a YAMS `SwerveDrive` for you — you only write the
subsystem and control bindings around it.

## Create a swerve subsystem

```java
import edu.wpi.first.math.geometry.Pose2d;
import edu.wpi.first.math.geometry.Rotation2d;
import edu.wpi.first.math.kinematics.ChassisSpeeds;
import edu.wpi.first.wpilibj.Filesystem;
import edu.wpi.first.wpilibj2.command.Command;
import edu.wpi.first.wpilibj2.command.SubsystemBase;
import java.io.File;
import java.util.function.DoubleSupplier;
import swervelib.parser.SwerveParser;
import yams.mechanisms.config.SwerveDriveConfig;
import yams.mechanisms.swerve.SwerveDrive;
import yams.mechanisms.swerve.utility.SwerveInputStream;
import yams.motorcontrollers.SmartMotorControllerConfig.TelemetryVerbosity;

public class SwerveDriveSubsystem extends SubsystemBase
{

  private SwerveDrive drive;

  public SwerveDriveSubsystem()
  {
    var cfg = new SwerveDriveConfig()
        .withStartingPose(new Pose2d(3, 3, Rotation2d.kZero))
        .withSubsystem(this)
        .withTelemetry(TelemetryVerbosity.HIGH);
    try
    {
      drive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve/base"))
          .createSwerveDrive(cfg);
    } catch (Exception e)
    {
      throw new RuntimeException(e);
    }
  }

  public SwerveInputStream getAngularVelocityStream(DoubleSupplier x, DoubleSupplier y,
                                                    DoubleSupplier rot)
  {
    return new SwerveInputStream(drive, x, y, rot);
  }

  public Command drive(SwerveInputStream stream)
  {
    return drive.drive(() -> ChassisSpeeds.fromFieldRelativeSpeeds(stream.get(),
                                                                   new Rotation2d(drive.getGyroAngle())));
  }

  /** Zero the gyro heading. Bind this to a button combo for field recovery. */
  public Command zeroGyro()
  {
    return runOnce(() -> drive.zeroGyro());
  }

  @Override
  public void periodic()
  {
    drive.updateTelemetry();
  }

  @Override
  public void simulationPeriodic()
  {
    drive.simIterate();
  }
}
```

`SwerveParser` reads `swervedrive.json` and every file it references under `modules/`, resolves your
hardware through the matching vendor library, and hands back a fully constructed YAMS `SwerveDrive` —
there's no constants file to maintain by hand.

`TelemetryVerbosity.HIGH` is useful while bringing your robot up (it's what step 4's dashboard values
come from) but adds NetworkTables overhead — turn it down once you've finished tuning.

## Bind your controller

```java
import edu.wpi.first.wpilibj2.command.button.CommandXboxController;
import frc.robot.subsystems.swervedrive.SwerveDriveSubsystem;
import yams.mechanisms.swerve.utility.SwerveInputStream;

public class RobotContainer
{

  final CommandXboxController driverXbox = new CommandXboxController(0);

  private final SwerveDriveSubsystem swerve = new SwerveDriveSubsystem();

  private final SwerveInputStream driveAngularVelocity =
      swerve.getAngularVelocityStream(
                driverXbox::getLeftY,
                driverXbox::getLeftX,
                () -> driverXbox.getRawAxis(2))
            .withAllianceRelativeControl();

  public RobotContainer()
  {
    configureBindings();
  }

  private void configureBindings()
  {
    // Default drive command
    swerve.setDefaultCommand(swerve.drive(driveAngularVelocity));

    // Zero the gyro with Start + Back — use this if the field-relative heading drifts
    driverXbox.start().and(driverXbox.back()).onTrue(swerve.zeroGyro());
  }
}
```

{% hint style="info" %}
**Why bind `zeroGyro()` to a button combo?** Gyros can drift, or power on facing the wrong direction.
A button combo (Start + Back, or both bumpers) lets the driver instantly re-align field-relative
control without touching the Driver Station. Always bind this — it has saved matches.
{% endhint %}

## First drive

1. Deploy your code.
2. On blocks, enable the robot in Teleop and push the drive stick gently forward. All four wheels
   should point the same direction and the robot should attempt to move that way.
3. Rotate with the rotation axis. The robot should spin around its center, not drift while spinning.
4. If a module fights the others (spins to the wrong angle, or drives backward relative to the rest),
   revisit [step 4](04-verify-and-calibrate-hardware.md) for that module's inversion/offset.
5. Once it drives cleanly on blocks, test on the ground at low speed before opening it up.

Congratulations — you have a driving swerve robot. From here, see the **How-to Guides** section for
[tuning PIDF gains](../how-to/tune-pidf-gains.md), diagnosing drift, and other tasks you'll
return to throughout the season.
