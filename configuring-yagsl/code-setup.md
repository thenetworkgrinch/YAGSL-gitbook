# Code Setup

## How to create a swerve drive?

YAGSL is unique in the fact that you can create a swerve drive based entirely off of JSON configuration files. The JSON configuration files should be located in the [`deploy`](https://github.com/BroncBotz3481/YAGSL-Example/src/main/deploy) directory. You can also create the Configuration objects manually and instantiate your Swerve Drive that way.

### How to create a SwerveDrive using JSON.

This example program creates the [`SwerveDrive`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/swervelib/SwerveDrive.java) in the [`SwerveSubsystem`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/java/frc/robot/subsystems/swervedrive2/SwerveSubsystem.java), as you should only interact with it in the SwerveSubsystem if you are using command based programming.

```java
import java.io.File;
import edu.wpi.first.wpilibj.Filesystem;
import swervelib.parser.SwerveParser;
import swervelib.SwerveDrive;
import edu.wpi.first.math.util.Units;


double maximumSpeed = Units.feetToMeters(4.5)
File swerveJsonDirectory = new File(Filesystem.getDeployDirectory(),"swerve");
SwerveDrive  swerveDrive = new SwerveParser(directory).createSwerveDrive(maximumSpeed);

```

This way is fast and easy, no more large unmaintainable and daunting constants file to worry about! To create a JSON directory look at the [configuration documentation](configuration/).
