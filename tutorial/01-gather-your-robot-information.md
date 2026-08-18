# 1. Gather your robot information

YAGSL is configured entirely from information about *your* robot — there is no universal constants
file that works for everyone. Before opening [config.yagsl.com](https://config.yagsl.com), collect
everything in this step. Having it all on hand up front saves you from re-measuring later.

{% hint style="warning" %}
Every swerve drive is different, even between robots using the same COTS modules (SDS MK4, MAXSwerve,
etc.). Don't assume your numbers match another team's example.
{% endhint %}

## Hardware types and connections

| Feature | Notes |
|---|---|
| Gyroscope model and connection | e.g. Pigeon 2.0 on CAN, NavX over USB/SPI/I2C, CANandGyro on CAN. The connection method matters — a NavX over USB is a different config than one on the MXP (SPI). |
| Drive motor + controller for each module | e.g. Kraken X60 on TalonFX, NEO on SparkMAX, etc. |
| Angle/steering motor + controller for each module | Same as above, per module. |
| Absolute encoder model and connection for each module | e.g. CANcoder on CAN, REV Through Bore on a data port, attached to a SparkMAX analog/duty-cycle port. |
| CAN bus name | `rio` on a roboRIO, unless you're running CTRE devices on a [CANivore](https://store.ctr-electronics.com/canivore/) (use the CANivore's configured name). Starting with the 2027 SystemCore, name whichever of its several native CAN buses each device is actually on. |
| CAN ID / channel of every motor controller, encoder, and gyro | Get this wrong and you'll be controlling the wrong device without realizing it. |

## Physical characteristics

| Feature | Relevance |
|---|---|
| Drive gear ratio | How many drive motor rotations produce one wheel rotation. Usually published by the module manufacturer. |
| Steering/angle gear ratio | How many steering motor rotations produce one full module rotation. Also published by the manufacturer. |
| Wheel diameter | Needed to convert motor rotations into distance traveled. |
| Module locations | Distance in inches from the robot's center to each module's center, split into "front" (X) and "left" (Y) components. See below for how to measure this. |

### Measuring module locations

Measure in CAD if you can, for precision — otherwise, use this technique from Team 5010:

1. Lay the robot on its side and rotate the wheels perpendicular to the direction you're measuring.
2. Measure to the *outside* of each wheel and subtract one wheel width to get the center-to-center
   measurement.
3. Repeat in the other direction.

Record all four module locations (front-left, front-right, back-left, back-right) relative to the
robot's center, in inches:

| Module | X "Front" (in) | Y "Left" (in) |
|---|---|---|
| Front Left (FL) | + | + |
| Front Right (FR) | + | − |
| Back Left (BL) | − | + |
| Back Right (BR) | − | − |

## Inversion and offsets — record placeholders now, fill in later

You won't be able to determine these until your code is deployed (that happens in
[step 4](04-verify-and-calibrate-hardware.md)), but it helps to have a table ready:

| Module | Drive Inverted? | Angle Inverted? | Absolute Encoder Inverted? | Absolute Encoder Offset (°) |
|---|---|---|---|---|
| Front Left (FL) | | | | |
| Front Right (FR) | | | | |
| Back Left (BL) | | | | |
| Back Right (BR) | | | | |

Also note whether your gyroscope needs to be inverted — it must report an **increasing** yaw while
the robot rotates counterclockwise.

{% hint style="info" %}
A printable version of a bring-up checklist covering all of the above (courtesy of DevilBotz 2876)
is available in the repository's asset folder if your team prefers a paper checklist during bring-up.
{% endhint %}

With this information in hand, move on to [installing YAGSL](02-install-yagsl.md).
