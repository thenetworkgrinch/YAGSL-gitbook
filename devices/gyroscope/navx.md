---
description: Calibrating a NavX 2
---

# NavX

The NavX2 is the latest iteration of the NavX series by Studica (formally by KauiLabs) and offers excellent readings and documentation on how to calibrate and really get the most out of your gyroscope.&#x20;

{% embed url="https://www.studica.ca/en/navx-2-mxp-robotics-navigation-sensor" %}

## Getting started

You now have a NavX, congratulations! Time to configure and calibrate it, if you do not follow these steps it will not work as good as possible.

## NavXUI Installation

Java 1.7 or higher is required

{% embed url="https://pdocs.kauailabs.com/navx-mxp/software/navx-mxp-ui/" %}

{% embed url="https://www.kauailabs.com/public_files/navx-mxp/navx-mxp.zip" %}
Installation ZIP
{% endembed %}

Useful for advanced calibration to ensure optimal accuracy on the robot.

## Calibration

Full documentation for calibration is here.

{% embed url="https://pdocs.kauailabs.com/navx-mxp/guidance/gyroaccelcalibration/" %}

We will provide a brief summary here.

Gyroscopes are sensitive to ambient temperature, so it is imperative that you recalibrate them upon reception **AND** at events. NavX instructions are as follows.

1. Place them NavX on a still platform parallel to the earth's surface (use a level!).
2. Press and hold the **CAL** button for at least 10 seconds.
3. When you release the **CAL** button ensure that the **CAL** led flashes briefly.
4. Press the **RESET** button.
5. Wait up to 15 seconds.
6. yaw accuracy may be diminished until the next On-the-fly Gyro calibration completes. (which means your first autonomous may not act right!)

## Connection Methods



