---
description: >-
  Advantage Scope is a data visualization tool, courtesy of team 6328 Mechanical
  Advantage, which can visualize the Swerve Drive to give you feedback for
  debugging.
---

# ❌ Advantage Scope

## Opening

Since the 2024 Season, [Advantage Scope](https://github.com/Mechanical-Advantage/AdvantageScope) has been included with the WPILib installation. There is no external download required, however with every WPILib update you should re-download the [WPILib installer](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html) to get the latest version of WPILib tools.

## Configuring Advantage Scope

1. Connect laptop to robot
2. Open `AdvantageScope (WPILib)` or in VS Code open the command palette and type in `WPILib: Start Tool` and click `AdvantageScope`
3. Click `Help` then `Show Preferences`&#x20;

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences.png" alt=""><figcaption><p>Advantage Scope's help menu</p></figcaption></figure>

4. Input the roboRIO IP address based off of your team number. `10.TE.AM.2`

<figure><img src="../.gitbook/assets/AdvantageScope-Preferences-IP.png" alt=""><figcaption><p>roboRIO Address field highlighted</p></figcaption></figure>

5. Connect to the robot (or the simulator)

<figure><img src="../.gitbook/assets/AdvantageScope-Connect.png" alt=""><figcaption><p>Connect to Robot menu</p></figcaption></figure>

6. Add a new tab by clicking the `+` on the right side of the window

<figure><img src="../.gitbook/assets/AdvantageScope-Add.png" alt=""><figcaption><p>Add new tab</p></figcaption></figure>

7. Add a new :crab: Swerve tab

<figure><img src="../.gitbook/assets/AdvantageScope-Swerve.png" alt=""><figcaption><p>Swerve tab</p></figcaption></figure>

8. Connect the module states and rotation from Smart Dashboard to the Fields.
   1. Under the `SmartDashboard/swerve` menu drag the `desiredStates` into the `States (Red)` field.
   2. From the same menu drag the `measuredStates` into the `States (Blue)` field.
   3. From the same menu drag the `robotRotation` into the `Robot Rotation` field.



<figure><img src="../.gitbook/assets/AdvantageScope-Swerve-Fields.png" alt=""><figcaption><p>Connecting data from network tables to the swerve fields</p></figcaption></figure>

9. Adjust the Data column to your Swerve Drive's properties.
   1. Under the `Data` column change the `Max Speed` field to the value of `SmartDashboard/swerve/maxSpeed` entry.&#x20;
   2. Change the `Rotation Units` to match what the `SmartDashboard/swerve/rotationUnit` shows.

<figure><img src="../.gitbook/assets/AdvantageScope-Swerve-Data.png" alt=""><figcaption><p>Change the swerve drive's data to match your robot</p></figcaption></figure>

10. (Optional) Adjust the Display column to accurately display your robot's chassis dimensions by changing the Size (Left-Right) and Size (Front-Back) to match what is under SmartDashboard/`swerve/sizeLeftRight` and `SmartDashboard/swerve/sizeFrontBack`

<figure><img src="../.gitbook/assets/AdvantageScope-Swerve-Display.png" alt=""><figcaption><p>Change the swerve's display parameters to represent your robot's actual dimensions</p></figcaption></figure>

## Overview

<figure><img src="../.gitbook/assets/FRC_web_component_snapshot.png" alt=""><figcaption><p>Swerve Drivebase while in motion with an incorrect configuration. <br>(Need to change this to an actual picture of Advantage Scope)</p></figcaption></figure>

{% hint style="info" %}
The **BLUE** lines are the measured velocity and position of the swerve module.

The **RED** lines is the velocity and position of the module sent!
{% endhint %}
