---
description: What is a Swerve Module?
---

# Swerve Modules

You may see a bunch of different classes in other teams code which represent a `SwerveModule` and wonder why there isn't a standard class. There is a very good reason for that, every motor, absolute encoder, gear ratio, and installation can be different! Some teams try to prevent more issue's in their Swerve Drive than others because they had more time to debug it. YAGSL aims to take into account all common issues and address them in JSON configuration files.

## Tips for building a Swerve Module

* If you are using a magnetic encoder ensure that it is glue'd correctly.

## What is in a Swerve Module?

* Drive Gears (ratio must be known)
* Steerinng Gears (ratio must be known)
* Drive Motor (+ controller)
* Angle/Azimuth/Steering Motor (+ controller)
* Absolute Encoder

## A review of a typical brushless motor controller

This may seem out of place but when debugging swerve drives this comes in handy very quickly!

Smart Motor Controllers typically have the following features

* Rotate in either direction.
* Have sensors on the motor that read in either direction.
* Burn up when jammed or can't move, causing very high amperage utilization.
* Drop the voltage level available when running.
* Need a minimum amount of voltage to turn against friction.
* Greased gears attaches to shaft.
* Ramp up to speed at a configurable rate to avoid using too much power instantly.
* Have integrated PID loops which can control the output based off of a connected sensors input (typically an encoder).
* Are connected to a CAN bus, by default the `rio` but if a [CANivore](https://store.ctr-electronics.com/canivore/) is connected it could be the name of the CANivore as long as the motor controller supports it.
* Rotate the wheel in more than one shaft rotation proportional to the gears.

All of these need to be set correctly in order to configure a Swerve Module properly. If one of these are not set correctly you might experience behavior that you won't easily be able to identify.
