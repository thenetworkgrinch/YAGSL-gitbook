# Preface

## Swerve Drives are complicated!

Swerve drives have many symptoms that are similar or the same and can be caused by many things. So debugging can be long and arduous if you don't have all the right information to interpret.

## Label a front and expect to change it.

It's easiest to label a front, maybe it's one that you want but expect to change it. Relative locations of modules are determined by the gyroscope. If your gyroscope is installed in the wrong orientation and can't be changed you WILL have to compensate for it in code that is not in any existing examples or apart of YAGSL.&#x20;

## Physically Label each component with their ID's

There is no easier way to screw up configuring a swerve drive (or any other kind of drive system) than putting the wrong channel number, CAN ID, or even CAN bus (if your device is on a CANivore) for a component whether it is the drive motor, angle motor, absolute encoder, or gyroscope. Some components don't have ID's like NavX's and instead have a few different `type`'s which define the connection method (`navx_spi`, `navx_usb`, `navx_i2c`) these just have to be known beforehand.

I strongly suggest you find the information from the [getting-to-know-your-robot](../configuring-yagsl/getting-to-know-your-robot/ "mention")sheet before you start configuring your robot.

## As much as you hope, plead, pray, or offer sacrifices a swerve drive will not always "Just work"

Some tools like the [CTRE Tuner X Swerve Generator](https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/tuner-swerve/index.html) use desktop client's to configure the swerve drive constants file for you based off of actions and preset data from COTS modules. These go a long way and are completely different that what YAGSL offers (and is alot more work than I can justify to create in my free time). You are going to need a decent understanding of WPILib and Java to consider programming a swerve drive correctly. Some examples of what you should know how to do are change controllers from [`XboxController`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/XboxController.html) to [`Joystick`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/Joystick.html) and do not assume just because a function is named `getRightX()` it actually get's the correct axis!&#x20;

