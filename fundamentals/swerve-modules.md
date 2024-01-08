---
description: What is a Swerve Module?
---

# Swerve Modules

You may see a bunch of different classes in other teams code which represent a `SwerveModule` and wonder why there isn't a standard class. There is a very good reason for that, every motor, absolute encoder, gear ratio, and installation can be different! Some teams try to prevent more issue's in their Swerve Drive than others because they had more time to debug it. YAGSL aims to take into account all common issues and address them in JSON configuration files.

{% hint style="warning" %}
If you are using a magnetic encoder ensure that the magnet is glued correctly so it does not slip.
{% endhint %}

## What is in a Swerve Module?

* [ ] Drive Gears (ratio must be known)
* [ ] Steering Gears (ratio must be known)
* [ ] Drive Motor (+ controller)
* [ ] Angle/Azimuth/Steering Motor (+ controller)
* [ ] Absolute Encoder

## Review

This may seem out of place but when debugging swerve drives this comes in handy very quickly!

Smart Motor Controllers typically have the following features

* [ ] Rotate in either direction.
* [ ] Have sensors on the motor that read in either direction.
* [ ] Burn up when jammed or can't move, causing very high amperage utilization.
* [ ] Drop the voltage level available when running.
* [ ] Need a minimum amount of voltage to turn against friction.
* [ ] Greased gears attaches to shaft.
* [ ] Ramp up to speed at a configurable rate to avoid using too much power instantly.
* [ ] Have integrated PID loops which can control the output based off of a connected sensors input (typically an encoder).
* [ ] Are connected to a CAN bus, by default the `rio` but if a [CANivore](https://store.ctr-electronics.com/canivore/) is connected it could be the name of the [CANivore](https://store.ctr-electronics.com/canivore/) as long as the motor controller supports it.
* [ ] Rotate the wheel in more than one shaft rotation proportional to the gears.

All of these need to be set correctly in order to configure a Swerve Module properly. If one of these are not set correctly you might experience behavior that you won't easily be able to identify.

#### TL;DR

1. Motors can break in many ways and are only expected to operate in one way, refer to here while debugging.
2. Swerve Modules contain, **drive gears**, **steering gears**, **drive motor**, **steering motor**, and a **absolute encoder**.

## Overview

Swerve Module's can get very complicated, very quickly. I will keep this example simple and may expand on it in the future. It will not include all of the steps above however there will be space for that in the constructor. For the purposes of this tutorial I will use [`CANSparkMax`](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkmax) from REVLib simply because it can be more verbose.

## Basics

Using REVLib we create a swerve module class the represents the module in code.

```java
import com.revrobotics.CANSparkMax;
import com.ctre.phoenix6.hardware.CANcoder;


public class SwerveModule {

    private CANSparkMax driveMotor;
    private CANSparkMax steerMotor;
    private CANcoder    absoluteEncoder;
    
    public SwerveModule(int driveMotorCANID, int steerMotorCANID, int cancoderCANID)
    {
        driveMotor = new CANSparkMax(driveMotorCANID);
        steerMotor = new CANSparkMax(steerMotorCANID);
        absoluteEncoder = new CANcoder(cancoderCANID);
        
        // Reset everything to factory default
        driveMotor.restoreFactoryDefaults();
        steerMotor.restoreFactoryDefaults();
        absoluteEncoder.getConfigurator().apply(new CANcoderConfiguration());
        
        // Continue configuration here..
        
    }

}
```

The class simply implements the drive and steering motors and the absolute encoder. This is the very basic constructor. If everything is configured via the hardware clients you do not need to restore the motors and absolute encoders to factory default, however this is not recommended.

## Configuration

Configuring a swerve module is EXTREMELY IMPORTANT!! It requires alot of patience and you will likely never get it working on the first try. I will be going through the required steps here.

### Absolute Encoder Offsets

Unless your mechanical team is working with extreme precision and places the magnet perfectly centered with the swerve module wheel already straight for every single module, you will need to set an offset for each module that way the absolute encoder reads the wheel orientation correctly.

You can and should get this value using a hardware client but if you don't want to you can always write a simple program like bellow to print out what the current value of the absolute encoder is.

```java
// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

package frc.robot;

import com.ctre.phoenix6.hardware.CANcoder;
import com.ctre.phoenix6.StatusSignal;
import com.ctre.phoenix6.signals.AbsoluteSensorRangeValue;
import com.ctre.phoenix6.signals.SensorDirectionValue;
import com.ctre.phoenix6.configs.CANcoderConfiguration;
import com.ctre.phoenix6.configs.CANcoderConfigurator;
import com.ctre.phoenix6.configs.MagnetSensorConfigs;
import edu.wpi.first.math.util.Units;


/**
 * The VM is configured to automatically run this class, and to call the functions corresponding to each mode, as
 * described in the TimedRobot documentation. If you change the name of this class or the package after creating this
 * project, you must also update the build.gradle file in the project.
 */
public class Robot extends TimedRobot
{

  private static Robot   instance;
  private CANcoder    absoluteEncoder;

  public Robot()
  {
    instance = this;
  }

  public static Robot getInstance()
  {
    return instance;
  }

  /**
   * This function is run when the robot is first started up and should be used for any initialization code.
   */
  @Override
  public void robotInit()
  {
   absoluteEncoder = new CANcoder(/* Change this to the CAN ID of the CANcoder */ 0);
   CANcoderConfigurator cfg = encoder.getConfigurator();
   cfg.apply(new CANcoderConfiguration());
   MagnetSensorConfigs  magnetSensorConfiguration = new MagnetSensorConfigs();
   cfg.refresh(magnetSensorConfiguration);
   cfg.apply(magnetSensorConfiguration
                  .withAbsoluteSensorRange(AbsoluteSensorRangeValue.Unsigned_0To1)
                  .withSensorDirection(SensorDirectionValue.CounterClockwise_Positive));
                  
  }

  /**
   * This function is called every 20 ms, no matter the mode. Use this for items like diagnostics that you want ran
   * during disabled, autonomous, teleoperated and test.
   *
   * <p>This runs after the mode specific periodic functions, but before LiveWindow and
   * SmartDashboard integrated updating.
   */
  @Override
  public void robotPeriodic()
  {
   StatusSignal<Double> angle = encoder.getAbsolutePosition().waitForUpdate(0.1);

   System.out.println("Absolute Encoder Angle (degrees): " + Units.rotationsToDegrees(angle.getValue()));
  }
}

```

### Inversion

Depending on your swerve module your motors may need to be inverted to run as expected.

```java
import com.revrobotics.CANSparkMax;
import com.ctre.phoenix6.hardware.CANcoder;
import com.ctre.phoenix6.StatusSignal;
import com.ctre.phoenix6.signals.AbsoluteSensorRangeValue;
import com.ctre.phoenix6.signals.SensorDirectionValue;
import com.ctre.phoenix6.configs.CANcoderConfiguration;
import com.ctre.phoenix6.configs.CANcoderConfigurator;
import com.ctre.phoenix6.configs.MagnetSensorConfigs;
import edu.wpi.first.math.util.Units;


public class SwerveModule {

    private CANSparkMax driveMotor;
    private CANSparkMax steerMotor;
    private CANcoder    absoluteEncoder;
    
    public SwerveModule(int driveMotorCANID, int steerMotorCANID, int cancoderCANID)
    {
        driveMotor = new CANSparkMax(driveMotorCANID);
        steerMotor = new CANSparkMax(steerMotorCANID);
        absoluteEncoder = new CANcoder(cancoderCANID);
        
        // Reset everything to factory default
        driveMotor.restoreFactoryDefaults();
        steerMotor.restoreFactoryDefaults();
        absoluteEncoder.getConfigurator().apply(new CANcoderConfiguration());
        
        // Continue configuration here..
        
        // CANcoder Configuration
        CANcoderConfigurator cfg = encoder.getConfigurator();
        cfg.apply(new CANcoderConfiguration());
        MagnetSensorConfigs  magnetSensorConfiguration = new MagnetSensorConfigs();
        cfg.refresh(magnetSensorConfiguration);
        cfg.apply(magnetSensorConfiguration
                  .withAbsoluteSensorRange(AbsoluteSensorRangeValue.Unsigned_0To1)
                  .withSensorDirection(SensorDirectionValue.CounterClockwise_Positive));

        // Steering Motor Configuration
        steerMotor.setInverted(false);
        
        // Drive Motor Configuration
        driveMotor.setInverted(false);
    }

}
```

### Conversion Factor

Math time! Remember dimensional analysis?

{% embed url="https://youtu.be/hIAdCTNi1S8" %}
Kahn Academy Explanation of Dimensional Analysis for unit conversions
{% endembed %}

Swerve Modules are given a [`SwerveModuleState`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/SwerveModuleState.html) object to set the velocity (Meters per Second) and a Angle (which we will give as degrees for simplicity) of the module. Which means we must convert native units (in our case rotations, and rotations per minute) to the velocity and angle units. We also know the gear ratio of which the motor runs to get a complete wheel rotation.

For this example we will assume the gear ratio is `12.8:1` [(SDS MK4 Steering Ratio)](https://www.swervedrivespecialties.com/collections/kits/products/mk4-swerve-module) which means the rotor spins `12.8` times to complete one mechanism rotation.

$$
SteeringConverionFactor = \frac{1_{degree}}{1_{rot}} = \frac{1_{rot}}{12.8_{rot}} * \frac{1_{rot}}{360_{deg}}
$$



For the following example we will use the gear ratio `6.75:1` [(SDS MK4 L2 Drive Ratio)](https://www.swervedrivespecialties.com/collections/kits/products/mk4-swerve-module) which means the rotor spins `6.75` times for the wheel to complete a rotation.

$$
DriveConversionFactor = \frac{\frac{1_{meter}}{1_{sec}}}{\frac{1_{rot}}{1_{min}}} = \frac{1_{rot}}{1_{min}} * \frac{1_{rot}}{6.75_{rot}} * \frac{60_{sec}}{1_{min}} * \frac{\pi*d_{meters}}{1_{rot}}
$$

All of this is the long way to show you that math is important and your conversion factors are not magic numbers!!

### PID Control

PID stands for Percent-Integral-Derivative. Swerve Drive's should try to use the most up to date feedback sensors so we will be using the on-board PID feature of the SparkMAX.&#x20;

WPILib has a great guide to learning PID's here. The turret position example is how we will control the steering motors.

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/introduction-to-pid.html" %}

REV's documentation on this is available and shows how it works (mostly) on the SparkMAX controller.

{% embed url="https://docs.revrobotics.com/sparkmax/operating-modes/closed-loop-control" %}

There are 2 PID's you need to control create to control a Swerve Module one for the drive motor, the other for the steering motors.&#x20;

#### Drive Motor PID

Based off of the example in WPILib docs, you just have to tune it as if it were a fly-wheel which is documented here.

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-flywheel.html" %}

For a drive wheel there may be a feedforward you want to always use to ensure the PID doesn't have adjust above the minimum voltage that would move the wheel with the weight of the robot ontop of it.

#### Steering Motor PID

The Steering Motor PID controls the angle of the wheel in degrees based off of the feedback sensor. A few tricks are necessary to do this effectively though.&#x20;

1. PID Wrapping ensures the wheel will always choose the shortest path to the destination angle.
2. Grease your gears often!!!
3. Calculate the correct conversion factor.
4. Tune quickly and accurately using the hardware client or tuner x.

WPILib has some documentation on a turret position controller which is the exact same principle.

{% embed url="https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-turret.html" %}

### Current Limiting

You must limit the currents of your motors to avoid pulling too much power and browning out, typically these are in the ranges of 20A for steering motors, and 40A for drive motors.

## Example Code

```java
import com.revrobotics.CANSparkMax;
import com.ctre.phoenix6.hardware.CANcoder;
import com.ctre.phoenix6.StatusSignal;
import com.ctre.phoenix6.signals.AbsoluteSensorRangeValue;
import com.ctre.phoenix6.signals.SensorDirectionValue;
import com.ctre.phoenix6.configs.CANcoderConfiguration;
import com.ctre.phoenix6.configs.CANcoderConfigurator;
import com.ctre.phoenix6.configs.MagnetSensorConfigs;
import edu.wpi.first.math.util.Units;
import com.revrobotics.RelativeEncoder;
import com.revrobotics.SparkMaxPIDController;


public class SwerveModule {

    private CANSparkMax driveMotor;
    private CANSparkMax steerMotor;
    private CANcoder    absoluteEncoder;
    private SparkMaxPIDController drivingPIDController;
    private SparkMaxPIDController turningPIDController;
    private RelativeEncoder driveEncoder;
    private RelativeEncoder steerEncoder;
    
    public SwerveModule(int driveMotorCANID, int steerMotorCANID, int cancoderCANID)
    {
        driveMotor = new CANSparkMax(driveMotorCANID);
        steerMotor = new CANSparkMax(steerMotorCANID);
        absoluteEncoder = new CANcoder(cancoderCANID);
        
        // Get the PID Controllers
        drivingPIDController = driveMotor.getPIDController();
        turningPIDController = steerMotor.getPIDController();
        
        // Get the encoders
        driveEncoder = driveMotor.getEncoder():
        steerEncoder = steerMotor.getEncoder();
        
        // Reset everything to factory default
        driveMotor.restoreFactoryDefaults();
        steerMotor.restoreFactoryDefaults();
        absoluteEncoder.getConfigurator().apply(new CANcoderConfiguration());
        
        // Continue configuration here..
        
        // CANcoder Configuration
        CANcoderConfigurator cfg = encoder.getConfigurator();
        cfg.apply(new CANcoderConfiguration());
        MagnetSensorConfigs  magnetSensorConfiguration = new MagnetSensorConfigs();
        cfg.refresh(magnetSensorConfiguration);
        cfg.apply(magnetSensorConfiguration
                  .withAbsoluteSensorRange(AbsoluteSensorRangeValue.Unsigned_0To1)
                  .withSensorDirection(SensorDirectionValue.CounterClockwise_Positive));

        // Steering Motor Configuration
        steerMotor.setInverted(false);
        turningPIDController.setFeedbackDevice(steerEncoder);
        // Apply position and velocity conversion factors for the turning encoder. We
        // want these in radians and radians per second to use with WPILib's swerve
        // APIs.
        steerEncoder.setPositionConversionFactor(ModuleConstants.kTurningEncoderPositionFactor);
        steerEncoder.setVelocityConversionFactor(ModuleConstants.kTurningEncoderVelocityFactor);
        // Enable PID wrap around for the turning motor. This will allow the PID
        // controller to go through 0 to get to the setpoint i.e. going from 350 degrees
        // to 10 degrees will go through 0 rather than the other direction which is a
        // longer route.
        turningPIDController.setPositionPIDWrappingEnabled(true);
        turningPIDController.setPositionPIDWrappingMinInput(0);
        turningPIDController.setPositionPIDWrappingMaxInput(90);
        // Set the PID gains for the turning motor. Note these are example gains, and you
        // may need to tune them for your own robot!
        turningPIDController.setP(ModuleConstants.kTurningP);
        turningPIDController.setI(ModuleConstants.kTurningI);
        turningPIDController.setD(ModuleConstants.kTurningD);
        turningPIDController.setFF(ModuleConstants.kTurningFF);
        
        // Drive Motor Configuration
        driveMotor.setInverted(false);
        drivingPIDController.setFeedbackDevice(driveEncoder);
        // Apply position and velocity conversion factors for the driving encoder. The
        // native units for position and velocity are rotations and RPM, respectively,
        // but we want meters and meters per second to use with WPILib's swerve APIs.        
        driveEncoder.setPositionConversionFactor(ModuleConstants.kDrivingEncoderPositionFactor);        
        driveEncoder.setVelocityConversionFactor(ModuleConstants.kDrivingEncoderVelocityFactor);
        // Set the PID gains for the driving motor. Note these are example gains, and you
        // may need to tune them for your own robot!
        drivingPIDController.setP(ModuleConstants.kDrivingP);
        drivingPIDController.setI(ModuleConstants.kDrivingI);
        drivingPIDController.setD(ModuleConstants.kDrivingD);
        drivingPIDController.setFF(ModuleConstants.kDrivingFF);
        
        // Save the SPARK MAX configurations. If a SPARK MAX browns out during
        // operation, it will maintain the above configurations.
        driveMotor.burnFlash();
        steerMotor.burnFlash();
          
        driveEncoder.setPosition(0);
        steerEncoder.setPosition(encoder.getAbsolutePosition().refresh().getValue() * 360);
    }
    
    
    /**
    Get the distance in meters.
    */
    public double getDistance()
    {
        return driveEncoder.getPosition();
    }
    
    /**
    Get the angle.
    */
    public Rotation2d getAngle()
    {
          return Rotation2d.fromDegrees(steerEncoder.getPosition());
    }
    
    /**
    Set the swerve module state.
    @param state The swerve module state to set.
    */
    public void setState(SwerveModuleState state)
    {
          turningPIDController.setReference(state.angle.getDegrees(), ControlType.kPosition);
          drivingPIDController.setReference(state.speedMetersPerSecond, ControlType.kVelocity);
    }

}
```
