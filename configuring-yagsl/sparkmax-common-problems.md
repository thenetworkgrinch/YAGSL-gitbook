---
description: These usually show up in MAXSwerve robots..
---

# SparkMax and SparkFlex Common Problems

## SparkMAX Absolute Encoder Boards

The [SparkMAX Absolute Encoder board](https://www.revrobotics.com/rev-11-3326/) should be zip tied and hot glued to the SparkMAX for redundancy and to prevent the board from coming loose.

### Throughbore Connection to the absolute encoder board

The connector from the [SparkMax Absolute Encoder board](https://www.revrobotics.com/rev-11-3326/) and the [REV Throughbore](https://www.revrobotics.com/rev-11-1271/) should be hot glued down on **BOTH** connection points because these can come out easily!&#x20;

The wire should also be tensioned just right, if its over tensioned the wire mgiht not be fully in the connector and the data could be incomplete, corrupted, or unnavailable.

## Throughbore's not adjusting to the absolute encoder offset

{% hint style="danger" %}
It is no longer necessary to set you `factor` to `360` with attached absolute encoders, but doing so is explained below.
{% endhint %}

When you are using Absolute Encoders attached to the SparkMax via an Absolute Encoder board it normally identifies as a DutyCycleEncoder.

You can use this as the primary feedback device by setting the `factor` in `physicalproperties.json` to `360` **HOWEVER** when you do this the absolut encoder offset given is in the JSON **IS NOT** applied. So you should use [`SwerveDrive.useExternalFeedbackSensor()`](https://broncbotz.org/YAGSL-Lib/docs/swervelib/SwerveDrive.html#useExternalFeedbackSensor\(\)) which **WILL** set the encoder offset into the SparkMAX DutyCycleEncoder Offset Flash Config.

To reset the SparkMax DutyCycleEncoder Offset you should use [`SwerveDrive.useInternalFeedbackSensor()`](https://broncbotz.org/YAGSL-Lib/docs/swervelib/SwerveDrive.html#useInternalFeedbackSensor\(\)) which will set the Spark DutyCycleEncoder Offset to 0.

## Status Frame Error

Usually if there's a status frame error it means that your connection to the REV Throughbore is incomplete somehow, or the sensor cable from your motor is loose or broken!&#x20;

If you see this be sure to check **ALL SENSOR CABLES GOING INTO THAT SPARKMAX!**



