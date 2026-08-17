# How to set up FRC Web Components

FRC Web Components is an easy way to visualize the swerve drive and give helpful feedback when debugging.

## Download

Download and install it from the releases page:

{% embed url="https://github.com/frc-web-components/app/releases/latest" %}

## Configuring FRC Web Components

<figure><img src="../.gitbook/assets/yagsl_fwc.gif" alt=""><figcaption></figcaption></figure>

1. Connect your laptop to the robot.
2. Open "FRC Web Components".
3. Click "Settings".

<figure><img src="../.gitbook/assets/fwc_config6.png" alt=""><figcaption><p>FRC Web Components with Settings highlighted</p></figcaption></figure>

4. Input the roboRIO IP address based on your team number: `10.TE.AM.2`.

{% embed url="https://docs.wpilib.org/en/stable/docs/networking/networking-introduction/ip-configurations.html#te-am-ip-notation" %}

<figure><img src="../.gitbook/assets/fwc_conrfig5.png" alt=""><figcaption><p>Dashboard settings for FRC Web Components.</p></figcaption></figure>

5. Open the widget menu.

<figure><img src="../.gitbook/assets/fwc_config4.png" alt=""><figcaption><p>Press here.</p></figcaption></figure>

6. Select `Swerve Drivebase` and click `Append`.

<figure><img src="../.gitbook/assets/fwc_config3.png" alt=""><figcaption></figcaption></figure>

7. Click on the widget.
8. Click "Connect to data source...".

<figure><img src="../.gitbook/assets/fwc_config2.png" alt=""><figcaption></figcaption></figure>

9. Connect to `SmartDashboard/swerve` by selecting it here.

<figure><img src="../.gitbook/assets/FWC_config1.png" alt=""><figcaption></figcaption></figure>

10. Press "Close".

## Reading the display

<figure><img src="../.gitbook/assets/FRC_web_component_snapshot.png" alt=""><figcaption><p>Swerve drivebase while in motion with an incorrect configuration.</p></figcaption></figure>

{% hint style="warning" %}
The **BLUE** lines are the measured velocity and position of each swerve module.

The **RED** lines are the velocity and position each module was commanded to.
{% endhint %}

Large, persistent gaps between the red and blue lines usually point to a tuning issue — see [how to tune PIDF gains](tune-pidf-gains.md) and [how to diagnose swerve drive drift](diagnose-swerve-drift.md).
