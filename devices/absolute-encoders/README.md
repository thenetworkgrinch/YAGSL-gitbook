# Absolute Encoders

YAGSL supports most common FRC absolute encoders. Absolute encoders are required if you are using a brushed motor .

The absolute encoder value will show up in shuffleboard under `Module[...] Raw Absolute Encoder` and can be used to tune the `absoluteEncoderOffset` in module JSON configuration files under most circumstances.

## Absolute Encoder Checklist

* [ ] All Absolute Encoder's spin counterclockwise positive.
* [ ] Magnetic Absolute encoders have a good read on the magnet while still and in operation (while the robot moves).
* [ ] Absolute Encoders have unique CAN ID's or Analog Input Channel's.
* [ ] Absolute Encoder are defined with the correct ID or Analog Input Channel.

{% hint style="warning" %}
**IF** you are attaching the absolute encoder to the motor controller the gearing and counts per revolution are not relevant and should be `360` instead of the calculated conversion factor for the angle motor!
{% endhint %}

## Swerve Absolute Encoder Wrapper

YAGSL created wrappers over all supported Absolute Encoders to uniformly fetch and set data that is needed for a Swerve Module to operate. This wrapper is called [`SwerveAbsoluteEncoder`](https://broncbotz3481.github.io/YAGSL/swervelib/encoders/SwerveAbsoluteEncoder.html). All [`SwerveAbsoluteEncoder`](https://broncbotz3481.github.io/YAGSL/swervelib/encoders/SwerveAbsoluteEncoder.html)'s can be fetched via the [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html#configuration) configuration object [`SwerveModuleConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html) absolute encoder attribute [`absoluteEncoder`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#absoluteEncoder). The [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html) is able to be fetched by [`SwerveDrive.getModules()`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html#getModules\(\)) easily.

YAGSL created wrappers over all supported Motor Controllers to uniformly fetch and set data that is needed for a Swerve Drive to operate. This wrapper is called [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html). All [`SwerveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/motors/SwerveMotor.html)'s can be fetched via the [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html#configuration) configuration object [`SwerveModuleConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html) motor definitions [`angleMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#angleMotor) and [`driveMotor`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModuleConfiguration.html#driveMotor). The [`SwerveModule`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveModule.html) is able to be fetched by [`SwerveDrive.getModules()`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html#getModules\(\)) easily.

<pre class="language-java"><code class="lang-java">import com.ctre.phoenix6.hardware.CANcoder;
 
   /**
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
      <a data-footnote-ref href="#user-content-fn-1">CANcoder</a> absoluteEncoder = (<a data-footnote-ref href="#user-content-fn-2">CANcoder</a>)m.configuration.absoluteEncoder.getAbsoluteEncoder();
    }
  }
</code></pre>

## Absolute Encoder Configuration

Inside any module JSON such as `frontleft.json`,`frontright.json`,`backleft.json`,`backright.json` this is what you would see to configure a absolute encoder.

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": <a data-footnote-ref href="#user-content-fn-3">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-4">5</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-5">null</a>
  },
  "angle": {
    "type": <a data-footnote-ref href="#user-content-fn-6">"sparkmax"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-7">6</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-8">null</a>
  },
  "encoder": {
    "type": <a data-footnote-ref href="#user-content-fn-9">"cancoder"</a>,
    "id": <a data-footnote-ref href="#user-content-fn-10">11</a>,
    "canbus": <a data-footnote-ref href="#user-content-fn-11">null</a>
  },
  "inverted": {
    "drive": <a data-footnote-ref href="#user-content-fn-12">false</a>,
    "angle": <a data-footnote-ref href="#user-content-fn-13">false</a>
  },
  "absoluteEncoderOffset": <a data-footnote-ref href="#user-content-fn-14">-18.281</a>,
  "location": {
    "front": <a data-footnote-ref href="#user-content-fn-15">-12</a>,
    "left": <a data-footnote-ref href="#user-content-fn-16">-12</a>
  }
}
</code></pre>

[^1]: [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) from Phoenix 6, initialized via CAN bus and CAN ID from configurations.

[^2]: [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) from Phoenix 6, initialized via CAN bus and CAN ID from configurations. Cast `Object` to [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html)

[^3]: SparkMAX brushless mode is selected.

[^4]: The SparkMAX has a CAN ID of `5`.

[^5]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^6]: SparkMAX brushless mode is selected.

[^7]: The SparkMAX has a CAN ID of `6`.

[^8]: SparkMAX is not compatible with CANivore so the `canbus` should be `null` or `""`.

[^9]: [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) is selected using the CAN ID and bus bellow.

[^10]: [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) id for this module.

[^11]: CANivore name of which the [`CANcoder`](https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/CANcoder.html) resides on.

[^12]: The drive motor spins counter clockwise positive without any inversion.

[^13]: The steering/angle/azimuth motor spins counterclockwise positive without inversion.

[^14]: Offset read from `Module[...] Raw Absolute Encoder` when the wheel was facing straight and bevels the same way.\
    \
    Offsets are in degrees.

[^15]: The center of this module is `-12`in from the center of the robot "frontwise".

[^16]: The center of this module is `-12`in from the center of the robot "left".
