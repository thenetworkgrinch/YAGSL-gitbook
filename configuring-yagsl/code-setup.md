# Code Setup

## Import YAGSL

### Online

Use WPILib vendor deps to install it.

{% embed url="https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#rd-party-libraries" %}

The URL for YAGSL vendordep is

```
https://broncbotz3481.github.io/YAGSL-Lib/yagsl/yagsl.json
```

### Offline

Copy the [`swervelib` directory from YAGSL-Example](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java) into `src/main/java` into your project.

{% hint style="warning" %}
You cannot have both the offline and online versions installed at the same time! Errors will occur!
{% endhint %}

{% hint style="info" %}
Be sure to [install all dependencies](dependency-installation.md) too!
{% endhint %}

## How to create a swerve drive?

YAGSL is unique in the fact that you can create a swerve drive based entirely off of JSON configuration files. The JSON configuration files should be located in the [`deploy`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/deploy/swerve/neo) directory. You can also create the Configuration objects manually and instantiate your Swerve Drive that way.

### How to create a SwerveDrive using JSON.

This example program creates the [`SwerveDrive`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html) in the [`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/blob/main/src/main/java/frc/robot/subsystems/swervedrive/SwerveSubsystem.java), as you should only interact with it in the [`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/blob/main/src/main/java/frc/robot/subsystems/swervedrive/SwerveSubsystem.java) if you are using command based programming.

<pre class="language-java"><code class="lang-java">import java.io.File;
import edu.wpi.first.wpilibj.Filesystem;
import swervelib.parser.SwerveParser;
import swervelib.SwerveDrive;
import edu.wpi.first.math.util.Units;


double <a data-footnote-ref href="#user-content-fn-1">maximumSpeed </a>= Units.feetToMeters(4.5)
File swerveJsonDirectory = new File(Filesystem.getDeployDirectory(),"swerve");
SwerveDrive  swerveDrive = new SwerveParser(directory).createSwerveDrive(<a data-footnote-ref href="#user-content-fn-2">maximumSpeed</a>);

</code></pre>

This way is fast and easy, no more large unmaintainable and daunting constants file to worry about! To create a JSON directory look at the [configuration documentation](configuration/).

## Telemetry

Telemetry can be great when you want it and YAGSL has no shortage of useful telemetry, such as the `Module[...]` fields you will see in Shuffleboard or SmartDashboard. However Telemetry causes delays and slowdowns to the program so sometimes it is best to the them off. To do this change

<pre class="language-java"><code class="lang-java">import swervelib.telemetry.SwerveDriveTelemetry;
import swervelib.telemetry.SwerveDriveTelemetry.TelemetryVerbosity;

<a data-footnote-ref href="#user-content-fn-3">SwerveDriveTelemetry.verbosity</a> = <a data-footnote-ref href="#user-content-fn-4">TelemetryVerbosity.HIGH</a>;
</code></pre>

## Follow along the example here!

{% embed url="https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/frc/robot" %}

Sometimes I like to include really advanced features in the example (like last year I had a drive to point command) so be sure to check back and see what we have done!

## Drive Code

Inside the `SwerveSubsystem` you can make your own drive code as easy as a few lines.

```java
  /**
   * Command to drive the robot using translative values and heading as a setpoint.
   *
   * @param translationX Translation in the X direction.
   * @param translationY Translation in the Y direction.
   * @param headingX     Heading X to calculate angle of the joystick.
   * @param headingY     Heading Y to calculate angle of the joystick.
   * @return Drive command.
   */
  public Command driveCommand(DoubleSupplier translationX, DoubleSupplier translationY, DoubleSupplier headingX,
                              DoubleSupplier headingY)
  {
    return run(() -> {
      // Make the robot move
      driveFieldOriented(getTargetSpeeds(translationX.getAsDouble(), translationY.getAsDouble(),
                                         headingX.getAsDouble(),
                                         headingY.getAsDouble()));
    });
  }

  /**
   * Command to drive the robot using translative values and heading as angular velocity.
   *
   * @param translationX     Translation in the X direction.
   * @param translationY     Translation in the Y direction.
   * @param angularRotationX Rotation of the robot to set
   * @return Drive command.
   */
  public Command driveCommand(DoubleSupplier translationX, DoubleSupplier translationY, DoubleSupplier angularRotationX)
  {
    return run(() -> {
      // Make the robot move
      swerveDrive.drive(new Translation2d(translationX.getAsDouble() * maximumSpeed, translationY.getAsDouble()),
                        angularRotationX.getAsDouble() * swerveDrive.swerveController.config.maxAngularVelocity,
                        true,
                        false);
    });
  }
```

[^1]: Maximum speed **MUST** be in Meters!

[^2]: Maximum speed **MUST** be in Meters!

[^3]: This [value ](https://broncbotz3481.github.io/YAGSL/swervelib/telemetry/SwerveDriveTelemetry.html#verbosity)is static and changes the telemetry given to the DriverStation and SmartDashboard.

[^4]: [Telemetry Verbosity](https://broncbotz3481.github.io/YAGSL/swervelib/telemetry/SwerveDriveTelemetry.TelemetryVerbosity.html) comes in several different modes.
