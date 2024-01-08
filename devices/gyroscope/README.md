# Gyroscope

YAGSL supports a wide variety of Gyroscopes in an effort to best help teams of all budgets, while we do recommend the NavX or Pigeon2 and officially support and have tested them, we do try to support others with varying degrees of success.&#x20;

## Swerve IMU wrapper

YAGSL creates wrappers over all supported Gyroscope types to uniformly fetch and set data that is needed for a Swerve Drive to operate, this wrapper is called [`SwerveIMU`](https://broncbotz3481.github.io/YAGSL/swervelib/imu/SwerveIMU.html). All Gyroscope's can be fetched from the [`SwerveDrive`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html#swerveDriveConfiguration) object via the [`SwerveDriveConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveDriveConfiguration.html) object which is generated from the `swervedrive.json` file given. For a user program to fetch the raw gyroscope object all you must do is as follows while casting the object to the right type, in our case this will be a NavX `AHRS` class.

<pre class="language-java"><code class="lang-java">   /**
   * Initialize {@link SwerveDrive} with the directory provided.
   *
   * @param directory Directory of swerve drive config files.
   */
  public SwerveSubsystem(File directory)
  {
    // Angle conversion factor is 360 / (GEAR RATIO * ENCODER RESOLUTION)
    //  In this case the gear ratio is 12.8 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double angleConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(12.8, 1);
    // Motor conversion factor is (PI * WHEEL DIAMETER IN METERS) / (GEAR RATIO * ENCODER RESOLUTION).
    //  In this case the wheel diameter is 4 inches, which must be converted to meters to get meters/second.
    //  The gear ratio is 6.75 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double driveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(4), 6.75, 1);

    // Configure the Telemetry before creating the SwerveDrive to avoid unnecessary objects being created.
    SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
    try
    {
      swerveDrive = new SwerveParser(directory).createSwerveDrive(maximumSpeed, angleConversionFactor, driveConversionFactor);
    } catch (Exception e)
    {
      throw new RuntimeException(e);
    }
    swerveDrive.setHeadingCorrection(false); // Heading correction should only be used while controlling the robot via angle.

<strong>    <a data-footnote-ref href="#user-content-fn-1">AHRS</a> navx = (<a data-footnote-ref href="#user-content-fn-2">AHRS</a>)swerveDrive.swerveDriveConfiguration.imu.getIMU();
</strong>
  }
</code></pre>

## Recommended Gyroscopes

These gyroscopes have been thoroughly tested and are used by many FRC teams. Generally speaking these will give you the best performance and reliability as well as help from the community at large.

### [NavX](navx.md)2 MXP by Studica

{% embed url="https://www.studica.ca/en/navx-2-mxp-robotics-navigation-sensor" %}

## Pigeon2 IMU



[^1]: NavX `AHRS` class which is used to represent and communicate with the NavX in Java.

[^2]: Casts java `Object` to `AHRS` class this should be changed to whatever the native class is for your Gyroscope.
