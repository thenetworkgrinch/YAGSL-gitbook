---
description: >-
  AdvantageScope is a data visualization tool, courtesy of team 6328 Mechanical
  Advantage, which can visualize the swerve drive to give you feedback for
  debugging.
---

# How to set up AdvantageScope

## Opening

Since the 2024 season, [AdvantageScope](https://github.com/Mechanical-Advantage/AdvantageScope) has been included with the WPILib installation. There is no external download required, but with every WPILib update you should re-download the [WPILib installer](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html) to get the latest version of WPILib tools.

## Configuring AdvantageScope

1. Connect your laptop to the robot.
2. Open `AdvantageScope (WPILib)`, or in VS Code open the command palette and type `WPILib: Start Tool`, then click `AdvantageScope`.
3. Click `Help`, then `Show Preferences`.

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences.png" alt=""><figcaption><p>AdvantageScope's help menu</p></figcaption></figure>

4. Input the roboRIO IP address based on your team number: `10.TE.AM.2`.

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences-IP.png" alt=""><figcaption><p>roboRIO Address field highlighted</p></figcaption></figure>

5. Connect to the robot (or the simulator).

<figure><img src="../.gitbook/assets/AdvantageScope-Connect.png" alt=""><figcaption><p>Connect to Robot menu</p></figcaption></figure>

6. Add a new tab by clicking the `+` on the right side of the window.

<figure><img src="../.gitbook/assets/AdvantageScope-Add.png" alt=""><figcaption><p>Add new tab</p></figcaption></figure>

7. Add a new 🦀 Swerve tab.

<figure><img src="../.gitbook/assets/AdvantageScope-Swerve.png" alt=""><figcaption><p>Swerve tab</p></figcaption></figure>

8. Connect the module states and rotation from SmartDashboard to the fields.
   1. Under the `SmartDashboard/swerve` menu, drag everything from `advantagescope/` into the `Sources` field.
   2. You may need to enable your robot before you can add `SmartDashboard/swerve/advantagescope/desiredStates`.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

9. Adjust the Data column to match your swerve drive's properties.
   1. Under the `Data` column, set `Max Speed` to the value of the `SmartDashboard/swerve/maxSpeed` entry.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

10. (Optional) Adjust the Display column to accurately reflect your robot's chassis dimensions by matching `Size (Left-Right)` and `Size (Front-Back)` to `SmartDashboard/swerve/sizeLeftRight` and `SmartDashboard/swerve/sizeFrontBack`.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## Reading the display

{% hint style="info" %}
The **RED** lines are the measured velocity and position of each swerve module.

The **BLUE** lines are the velocity and position each module was commanded to.
{% endhint %}

Large, persistent gaps between the red and blue lines usually point to a tuning issue — see [how to tune PIDF gains](tune-pidf-gains.md) and [how to diagnose swerve drive drift](diagnose-swerve-drift.md).
