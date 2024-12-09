# Motor Controllers

YAGSL supports most common FRC motor controllers (except [Venom](https://www.playingwithfusion.com/productview.php?pdid=99\&catid=1014)'s) and while we recommend using a brushless motor with an integrated encoder if at all possible we support brushed motors with external quadrature encoders as well **ONLY** with SparkMAX's.

The integrated encoder value will show up in shuffleboard under `swerve/modules/.../Raw Motor Encoder` as directly outputting the encoder value from the motor.

## Motor Checklist

* [ ] All motors spin counterclockwise positive, if not document inversions required to achieve that.
* [ ] PID is tuned to the best of your abilities.
* [ ] Motor is updated to the latest firmware.
* [ ] Motor has unique CAN ID.
* [ ] Motor type is set.
* [ ] CAN ID given corresponds to the one in the JSON module.
* [ ] Wheels are aligned forwards with all bevels facing the same way.

## Swerve Motor Wrapper

YAGSL created wrappers over all supported Motor Controllers to uniformly fetch and set data that is needed for a Swerve Drive to operate. This wrapper is called [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html). All [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html)'s can be fetched via the [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html#configuration) configuration object [`SwerveModuleConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html) motor definitions [`angleMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#angleMotor) and [`driveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#driveMotor). The [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html) is able to be fetched by [`SwerveDrive.getModules()`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html#getModules\(\)) easily.

<pre class="language-java" data-full-width="true"><code class="lang-java"> /**
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
      CANSparkMax steeringMotor = (<a data-footnote-ref href="#user-content-fn-1">CANSparkMax</a>)m.getAngleMotor().getMotor(); 
      CANSparkMax driveMotor = (<a data-footnote-ref href="#user-content-fn-2">CANSparkMax</a>)m.getDriveMotor().getMotor(); 
    }
  }
</code></pre>

## Motor Controller Configuration

{% hint style="warning" %}
Only CTRE devices currently support the `canbus` option, if your device is using the roboRIO `canbus` you must use the value of `null` or `"rio"` for supported CTRE devices. If you are using a CANivore, and the device is on the CANivore bus, the name must match the CANivore name.
{% endhint %}

Inside any module JSON such as `frontleft.json`,`frontright.json`,`backleft.json`,`backright.json` this is what you would see to configure a motor controller.

<pre class="language-json"><code class="lang-json">{
  "drive": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-3">"sparkmax"</a>,
</strong><strong>    "id": <a data-footnote-ref href="#user-content-fn-4">5</a>,
</strong><strong>    "canbus": <a data-footnote-ref href="#user-content-fn-5">null</a>
</strong>  },
  "angle": {
<strong>    "type": <a data-footnote-ref href="#user-content-fn-6">"sparkmax"</a>,
</strong><strong>    "id": <a data-footnote-ref href="#user-content-fn-7">6</a>,
</strong><strong>    "canbus": <a data-footnote-ref href="#user-content-fn-8">null</a>
</strong>  },
  "encoder": {
    "type": "cancoder",
    "id": 11,
    "canbus": null
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-9">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-10">false</a>
  },
  "absoluteEncoderOffset": -18.281,
  "location": {
    "front": <a data-footnote-ref href="#user-content-fn-11">-12</a>,
    "left": <a data-footnote-ref href="#user-content-fn-12">-12</a>
  }
}
</code></pre>

## &#x20;Possible Motor Controller Types

| Device                    | type                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------- |
| [SparkMAX](sparkmax.md)   | `sparkmax` (same as `neo`);`neo` ; `neo550`                                           |
| [SparkFlex](sparkflex.md) | `sparkflex` (same as `vortex`); `vortex`; `sparkflex_neo`; `sparkflex_vortex`         |
| [TalonFX](talonfx.md)     | `talonfx`(same as `krakenx60`);`falcon500`;`falcon500foc`;`krakenx60`; `krakenx60foc` |
| TalonSRX                  | `talonsrx`                                                                            |
| SparkMAX Brushed          | `sparkmax_brushed`                                                                    |

{% hint style="warning" %}
At this time I do not provide documentation on brushed motor utilization, like `sparkmax_brushed` and `talonsrx`.
{% endhint %}

[^1]: Cast the motor controller object that `SwerveMotor` wraps around back to the original class, in this case `CANSparkMax`

[^2]: Cast the motor controller object that `SwerveMotor` wraps around back to the original class, in this case `CANSparkMax`

[^3]: SparkMAX brushless mode is selected.

[^4]: The SparkMAX has a CAN ID of `5`.

[^5]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^6]: SparkMAX brushless mode is selected.

[^7]: The SparkMAX has a CAN ID of `6`.

[^8]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^9]: The drive motor spins counter clockwise positive without any inversion.

[^10]: The steering/angle/azimuth motor spins counterclockwise positive without inversion.

[^11]: The center of this module is `-12`in from the center of the robot "frontwise".

[^12]: The center of this module is `-12`in from the center of the robot "left".
