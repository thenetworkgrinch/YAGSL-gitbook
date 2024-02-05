# Check your motors

## Physically Label each component with their ID's

There is no easier way to screw up configuring a swerve drive (or any other kind of drive system) than putting the wrong channel number, CAN ID, or even CAN bus (if your device is on a CANivore) for a component whether it is the drive motor, angle motor, absolute encoder, or gyroscope. Some components don't have ID's like NavX's and instead have a few different `type`'s which define the connection method (`navx_spi`, `navx_usb`, `navx_i2c`) these just have to be known beforehand.

I strongly suggest you find the information from the [getting-to-know-your-robot](../configuring-yagsl/getting-to-know-your-robot/ "mention")sheet before you start configuring your robot.

## Motors should spin counter clockwise positive

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption><p>The left and right are physical left and right.</p></figcaption></figure>

When you spin your motor while the robot is disabled you will notice `Motor[...] Raw Angle Encoder` (angle/steering/azimuth relative encoder) and `Motor[...] Raw Absolute Encoder` (absolute encoder) . Both of these should increase while the motor is spun counter clockwise. For more information see here.

{% content-ref url="../configuring-yagsl/when-to-invert.md" %}
[when-to-invert.md](../configuring-yagsl/when-to-invert.md)
{% endcontent-ref %}

## Conversion Factors and your motors

Conversion factors are applied to your motor convert from native units (usually _**rotations**_) to degrees for steering/azimuth/angle motors, and meters for drive motors. Conversion factors are only relevant to motor controllers, except if there is an absolute encoder attached to your motor controller.

## The Absolute Encoder Offset

The absolute encoder offset is what allows your swerve module to maintain the wheel orientation between power offs. It is vital to a functioning swerve drive.

1. Line up all wheels so that the bevels are facing to the left like this.

<figure><img src="../.gitbook/assets/devilbots_cropped_swerve_orientation.png" alt=""><figcaption></figcaption></figure>

2. Deploy your code.
3. **DO NOT ENABLE YOUR ROBOT!**
4. Open shuffleboard.
5.

    <figure><img src="../.gitbook/assets/shuffleboard_open_tab.png" alt=""><figcaption><p>Open network tables</p></figcaption></figure>


6.

    <figure><img src="../.gitbook/assets/shuffleboard_read_vals.png" alt=""><figcaption></figcaption></figure>

7. Take note of the `Module[...] Raw Absolute Encoder` value's and use them for `absoluteEncoderOffset` in the module JSONs.

{% hint style="warning" %}
### Special note to MAXSwerve teams!

When you align using the MAXSwerveModules and find the `absoluteEncoderOffset` for each module you may need to **+/-`90` from the `absoluteEncoderOffset`** to correct the module position. The `absoluteEncoderInverted` must also be `true` in every module configuration as well.

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 12,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 11,
    "canbus": null
  },
  "encoder": {
    "type": "attached",
    "id": 0,
    "canbus": null
  },
  "inverted": {
    "drive": false,
<strong>    "angle": false
</strong>  },
<strong>  "absoluteEncoderInverted": true,
</strong>  "absoluteEncoderOffset": -90,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>
{% endhint %}
