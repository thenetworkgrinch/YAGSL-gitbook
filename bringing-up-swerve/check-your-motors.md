# Check your motors

## Physically Label each component with their ID's

There is no easier way to screw up configuring a swerve drive (or any other kind of drive system) than putting the wrong channel number, CAN ID, or even CAN bus (if your device is on a CANivore) for a component whether it is the drive motor, angle motor, absolute encoder, or gyroscope. Some components don't have ID's like NavX's and instead have a few different `type`'s which define the connection method (`navx_spi`, `navx_usb`, `navx_i2c`) these just have to be known beforehand.

I strongly suggest you find the information from the [getting-to-know-your-robot](../configuring-yagsl/getting-to-know-your-robot/ "mention")sheet before you start configuring your robot.

## Motors should spin counter clockwise positive

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Motors with purple bevels</p></figcaption></figure>

When you spin your motor while the robot is disabled you will notice `Motor[...] Raw Angle Encoder` (angle/steering/azimuth relative encoder) and `Motor[...] Raw Absolute Encoder` (absolute encoder) . Both of these should increase while the motor is spun counter clockwise. For more information see here.

{% content-ref url="../configuring-yagsl/when-to-invert.md" %}
[when-to-invert.md](../configuring-yagsl/when-to-invert.md)
{% endcontent-ref %}
