---
description: Calibrating a NavX 2
---

# NavX

The NavX2 is the latest iteration of the NavX series by Studica (formally by KauiLabs) and offers excellent readings and documentation on how to calibrate and really get the most out of your gyroscope.&#x20;

{% embed url="https://www.studica.ca/en/navx-2-mxp-robotics-navigation-sensor" %}

## Getting started

You now have a NavX, congratulations! Time to configure and calibrate it, if you do not follow these steps it will not work as good as possible.

The java class for the NavX is `AHRS` imported from `import com.kauailabs.navx.frc.AHRS;`

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

### SPI (`navx`, `navx_spi`)

YAGSL supports the NavX SPI communication over the roboRIO MXP. This is the recommended method of communicating with NavX devices.

### Serial (`navx_mxp`, `navx_usb`)

Serial communication is slower than SPI communication and could be more prone to interference however this is the only out of the box way to communicate with the [NavX2 micro](https://www.andymark.com/products/navx2-micro-navigation-sensor) (`navx_usb`) so this must be selected if you are using one. The MXP serial communication can be used incase the SPI MXP communication is not working at the moment.

### I2C

I2C communication on the MXP is supported however this is known to cause permanent lockouts on the roboRIO randomly, this issue has been thoroughly investigated and no known cause can be found. It is encouraged to avoid using this option at all costs!!

## NavX Micro

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>NavX micro from andymark</p></figcaption></figure>

If you are using the NavX micro please select the type of `navx_usb` to indicate you are communicating over serial via the USB or else the gyrscope will not work with YAGSL!
