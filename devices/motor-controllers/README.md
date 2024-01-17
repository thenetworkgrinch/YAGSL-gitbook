# Motor Controllers

YAGSL supports most common FRC motor controllers (except [Venom](https://www.playingwithfusion.com/productview.php?pdid=99\&catid=1014)'s) and while we recommend using a brushless motor with an integrated encoder if at all possible we support brushed motors with external quadrature encoders as well **ONLY** with SparkMAX's.

The integrated encoder value will show up in shuffleboard under `Module[...] Raw Motor Encoder` as directly outputting the encoder value from the motor.

## Motor Checklist

* [ ] All motors spin counterclockwise positive, if not document inversions required to achieve that.
* [ ] PID is tuned to the best of your abilities.
* [ ] Motor is updated to the latest firmware.
* [ ] Motor has unique CAN ID.
* [ ] CAN ID given corresponds to the one in the JSON module.
* [ ] Wheels are aligned forwards with all bevels facing the same way.

## Swerve Motor Wrapper

YAGSL created wrappers over all supported Motor Controllers to uniformly fetch and set data that is needed for a Swerve Drive to operate. This wrapper is called [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html). All [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html)'s can be fetched via the [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html#configuration) configuration object [`SwerveModuleConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html) motor definitions [`angleMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#angleMotor) and [`driveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#driveMotor). The [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html) is able to be fetched by [`SwerveDrive.getModules()`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html#getModules\(\)) easily.

<pre class="language-java"><code class="lang-java"> /**
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

    for(SwerveModule m : swerveDrive.getModules())
    {
      System.out.println("Module Name: "+m.configuration.name);
      <a data-footnote-ref href="#user-content-fn-1">CANSparkMax</a> steeringMotor = (<a data-footnote-ref href="#user-content-fn-2">CANSparkMax</a>)m.configuration.angleMotor.getMotor();
      <a data-footnote-ref href="#user-content-fn-3">CANSparkMax</a> driveMotor = (<a data-footnote-ref href="#user-content-fn-4">CANSparkMax</a>)m.configuration.driveMotor.getMotor();
    }
  }
</code></pre>

## Motor Controller Configuration

Inside any module JSON such as `frontleft.json`,`frontright.json`,`backleft.json`,`backright.json` this is what you would see to configure a motor controller.

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": <a data-footnote-ref href="#user-content-fn-5">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-6">5</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-7">null</a>
  },
  "angle": {
    "type": <a data-footnote-ref href="#user-content-fn-8">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-9">6</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-10">null</a>
  },
  "encoder": {
    "type": "cancoder",
    "id": 11,
    "canbus": null
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-11">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-12">false</a>
  },
  "absoluteEncoderOffset": -18.281,
  "location": {
    "front": <a data-footnote-ref href="#user-content-fn-13">-12</a>,
    "left": <a data-footnote-ref href="#user-content-fn-14">-12</a>
  }
}
</code></pre>

## &#x20;Possible Motor Controller Types

| Device                    | type               |
| ------------------------- | ------------------ |
| [SparkMAX](sparkmax.md)   | `sparkmax`         |
| [SparkFlex](sparkflex.md) | `sparkflex`        |
| [TalonFX](talonfx.md)     | `talonfx`          |
| TalonSRX                  | `talonsrx`         |
| SparkMAX Brushed          | `sparkmax_brushed` |

{% hint style="warning" %}
At this time I do not provide documentation on brushed motor utilization, like `sparkmax_brushed` and `talonsrx`.
{% endhint %}

[^1]: Type of motor controller that is being used.

[^2]: Cast the motor controller object that `SwerveMotor` wraps around back to the original class, in this case `CANSparkMax`

[^3]: Type of motor controller that is being used.

[^4]: Cast the motor controller object that `SwerveMotor` wraps around back to the original class, in this case `CANSparkMax`

[^5]: SparkMAX brushless mode is selected.

[^6]: The SparkMAX has a CAN ID of `5`.

[^7]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^8]: SparkMAX brushless mode is selected.

[^9]: The SparkMAX has a CAN ID of `6`.

[^10]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^11]: The drive motor spins counter clockwise positive without any inversion.

[^12]: The steering/angle/azimuth motor spins counterclockwise positive without inversion.

[^13]: The center of this module is `-12`in from the center of the robot "frontwise".

[^14]: The center of this module is `-12`in from the center of the robot "left".
