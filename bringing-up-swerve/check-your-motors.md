# ❌ Check your motors

## Physically Label each component with their ID's

There is no easier way to screw up configuring a swerve drive (or any other kind of drive system) than putting the wrong channel number, CAN ID, or even CAN bus (if your device is on a CANivore) for a component whether it is the drive motor, angle motor, absolute encoder, or gyroscope. Some components don't have ID's like NavX's and instead have a few different `type`'s which define the connection method (`navx_spi`, `navx_usb`, `navx_i2c`) these just have to be known beforehand.

I strongly suggest you find the information from the [getting-to-know-your-robot](../configuring-yagsl/getting-to-know-your-robot/ "mention")sheet before you start configuring your robot.
