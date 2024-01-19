# Configuration

## Tuning Webpage

YAGSL maintains a somewhat complete configuration tool to aid you in creating configuration files from scratch here.

{% embed url="https://broncbotz3481.github.io/YAGSL-Example/" %}

{% hint style="warning" %}
**IF** you are using an Absolute Encoder which is attached to your SparkMAX then the `angle` conversion factor is `360`.
{% endhint %}

From this webpage you should be able to download all files and place them into your `deploy` directory to use for [`new SwerveParser(new File(Filesystem.getDeployDirectory(),"swerve")`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveParser.html#%3Cinit%3E\(java.io.File\))[`.createSwerveDrive(Units.feetToMeters(4.5));`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveParser.html#createSwerveDrive\(double\)) .

We also maintain a repository for known working configurations here!

{% embed url="https://github.com/BroncBotz3481/YAGSL-Configs" %}

## Make sure you know what module is what!

If not your modules could end up like this!

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Modules flipped around</p></figcaption></figure>

## Configuration Directory

### JSON Files

JSON stands for "JavaScript Object Notation" it is a popular format for configuration, and string data representation. Learn more [here](https://www.w3schools.com/js/js\_json\_intro.asp)

### What does the swerve directory need to look like?

The swerve directory for generating a swerve drive must look like this. Assuming you have defined your swerve modules as `"modules": [ "frontleft.json", "frontright.json", "backleft.json", "backright.json"]` in [`swervedrive.json`](swerve-drive-configuration.md).

```
└── swerve
    ├── controllerproperties.json
    ├── modules
    │   ├── backleft.json
    │   ├── backright.json
    │   ├── frontleft.json
    │   ├── frontright.json
    │   ├── physicalproperties.json
    │   └── pidfproperties.json
    └── swervedrive.json
```

The `modules` folder, [`controllerproperties.json`](controller-properties-configuration.md), [`physicalproperties.json`](physical-properties-configuration.md), [`pidfproperties.json`](pidf-properties-configuration/), and [`swervedrive.json`](swerve-drive-configuration.md) files are necessary to build the swerve drive. Each one has specific fields which correspond to an attribute of the swerve drive, some physical and some abstract.

## Migrating Old Configuration Files

1. Delete `wheelDiamter`, `gearRatio`, `encoderPulsePerRotation` from `physicalproperties.json`
2. Add `optimalVoltage` to `physicalproperties.json`
3. Delete `maxSpeed` and `optimalVoltage` from `swervedrive.json`
4. **IF** a swerve module doesn't have the same drive motor or steering motor as the rest of the swerve drive you **MUST** specify a `conversionFactor` for BOTH the drive and steering motor in the modules configuration JSON file. IF one of the motors is the same as the rest of the swerve drive and you want to use that `conversionFactor`, set the `conversionFactor` in the module JSON configuration to 0.
5. You MUST specify the maximum speed when creating a `SwerveDrive` through `new SwerveParser(directory).createSwerveDrive(maximumSpeed);`
6. IF you do not want to set `conversionFactor` in `swervedrive.json`. You can pass it into the constructor as a parameter like this

```java
double DriveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(WHEEL_DIAMETER), GEAR_RATIO, ENCODER_RESOLUTION);
double SteeringConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(GEAR_RATIO, ENCODER_RESOLUTION);
new SwerveParser(directory).createSwerveDrive(maximumSpeed, SteeringConversionFactor, DriveConversionFactor);
```
