# Creating your first configuration

## DevilBotz 2876 Swerve Bring-Up Checklist

Compliments of the [DevilBotz 2876](https://www.thebluealliance.com/team/2876/2024)!

Updated: 2024-01-24

## Printable version

{% file src="../.gitbook/assets/2024-01-24 DevilBotz 2876 Swerve Bring-Up Checklist.pdf" %}

## Resources

* [ ] [YAGSL Wiki](https://yagsl.gitbook.io/yagsl/) - [https://yagsl.gitbook.io/yagsl/](https://yagsl.gitbook.io/yagsl/)
* [ ] [REV Robotics Hardware Client](https://docs.revrobotics.com/rev-hardware-client/) - [https://docs.revrobotics.com/rev-hardware-client/](https://docs.revrobotics.com/rev-hardware-client/)
  * _for configuring Spark Max Motor Controllers and other Rev devices_
* [ ] [Phoenix Tuner X](https://pro.docs.ctr-electronics.com/en/stable/docs/hardware-reference/cancoder/index.html) - [https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/index.html](https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/index.html)
  * _for configuring CanCoders and other CTR devices_

## Swerve Orientation Diagram

&#x20;_Note: When viewed from the top, make sure the sides of the wheel with the bevel gear are pointing to the **left**_

<figure><img src="swerve-orientation-diagram-devilbotz-2876.png" alt=""><figcaption></figcaption></figure>

### Step 1: Module Types

|                    | **Model, Version, Etc** |
| ------------------ | ----------------------- |
| _Motor_            |                         |
| _Controller_       |                         |
| _Absolute Encoder_ |                         |
| _IMU_              |                         |

### Step 2: Build Specific Details

1. Measure the module center relative to the **robot center**

| Module             | X "Front" (Inches) | Y "Left" (Inches) |
| ------------------ | ------------------ | ----------------- |
| _Front Left (FL)_  | +                  | +                 |
| _Front Right (FR)_ | +                  | -                 |
| _Back Left (BL)_   | -                  | +                 |
| _Back Right (BR)_  | -                  | -                 |

2. Measure the wheel diameter _in meters_
3. [Determine the _reported_ internal encoder resolution](#user-content-fn-1)[^1]
   * _Note: Most encoders now normalize the reported values to `-1` to `1`, so the Encoder Resolution when computing the conversion factors should generally be “`1`”. Only known exception is the TalonSRX._
4. Find the drive/angle gear ratio from the swerve module manufacturer specs
5. Calculate the [drive/angle conversion factors](#user-content-fn-2)[^2]
   * Drive Motor Conversion Factor (meters/rotation) = (PI \* WHEEL DIAMETER IN METERS) / (GEAR RATIO \* ENCODER RESOLUTION)
   * Angle Motor Conversion Factor (degrees/rotation) = 360 / (GEAR RATIO \* ENCODER RESOLUTION)

{% hint style="warning" %}
_Note: For Absolute Encoders attached **directly** to the dataport on the SparkMAX, the Conversion Factor is **`360`**_
{% endhint %}

| Motor   | Wheel Diameter (meters) | Gear Ratio | Encoder Resolution (CPR) | Conversion Factor |
| ------- | ----------------------- | ---------- | ------------------------ | ----------------- |
| _Drive_ |                         |            | 1                        |                   |
| _Angle_ | **N/A**                 |            | 1                        |                   |

### Step 3: Electrical Characteristics

* [ ] Set/Verify the CAN IDs for each module
* [ ] Check Inversion
  1. Rotate the _drive_ wheel **CCW** (moving “forward”)
     * [ ] The built-in encoder value should **increase**. If not, _invert_ the drive motor.
  2. Rotate the _angle_ wheel **CCW** (when viewed from the top)
     * [ ] The built-in encoder value should **increase**. If not, _invert_ the angle motor.
     * [ ] The absolute encoder value should **increase**. If not, _invert_ the absolute encoder.
  3. Rotate the entire _robot_ **CCW**. The gyro angle (yaw) should **increase**. If not, _invert_ the IMU

{% hint style="warning" %}
_Note: If you are using the hardware utilities for accessing the motors controllers and/or absolute encoders, the RoboRio must **not** be active on the CAN bus. The most reliable way to disable the RoboRio is to temporarily disconnect it from power._
{% endhint %}

| Module             | Motor CAN IDs | Motor CAN IDs | Encoder CAN/Channel ID |
| ------------------ | ------------- | ------------- | ---------------------- |
|                    | **Drive**     | **Angle**     | **Absolute Encoder**   |
| _Front Left (FL)_  |               |               |                        |
| _Front Right (FR)_ |               |               |                        |
| _Back Left (BL)_   |               |               |                        |
| _Back Right (BR)_  |               |               |                        |

| Module             | Inverted? |           |                      |         |
| ------------------ | --------- | --------- | -------------------- | ------- |
|                    | **Drive** | **Angle** | **Absolute Encoder** | **IMU** |
| _Front Left (FL)_  |           |           |                      |         |
| _Front Right (FR)_ |           |           |                      |         |
| _Back Left (BL)_   |           |           |                      |         |
| _Back Right (BR)_  |           |           |                      |         |

### Step 4: Absolute Encoder Offsets

1. Turn Robot On (Disabled so the wheels can be turned manually)
2. Manually Turn All 4 wheels so that they are all pointing forward and forward rotation results in increasing drive encoder values (see the black arrows in [Orientation Diagram](creating-your-first-configuration.md#swerve-orientation-diagram-1)).
3. Measure the absolute encoder value for each module (the offset is the **negative** of the value reported)

| Module             | Angle Absolute Offset |                     |
| ------------------ | --------------------- | ------------------- |
|                    | **(rotations 0-1)**   | **(degrees 0-360)** |
| _Front Left (FL)_  |                       |                     |
| _Front Right (FR)_ |                       |                     |
| _Back Left (BL)_   |                       |                     |
| _Back Right (BR)_  |                       |                     |

### Step 5: Input data into the configuration webpage

Open the webpage and import your data into the config files. \
[https://broncbotz3481.github.io/YAGSL-Example/](https://broncbotz3481.github.io/YAGSL-Example/)

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}



[^1]: IF you are using a SparkMAX or TalonFX this is going to be `1`!

[^2]: These convert rotations to meters/degrees for the motor controller.
