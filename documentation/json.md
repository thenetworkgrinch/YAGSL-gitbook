# JSON

## JSON Files

JSON stands for "JavaScript Object Notation" it is a popular format for configuration, and string data representation. Learn more [here](https://www.w3schools.com/js/js\_json\_intro.asp)

## What does the swerve directory need to look like?

The swerve directory for generating a swerve drive must look like this. Assuming you have defined your swerve modules as `"modules": [ "frontleft.json", "frontright.json", "backleft.json", "backright.json"]` in [`swervedrive.json`](https://github.com/BroncBotz3481/YAGSL-Example/tree/main/src/main/deploy/swerve/swervedrive.json).

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

The `modules` folder, `controllerproperties.json`, `physicalproperties.json`, `pidfproperties.json`, and `swervedrive.json` files are necessary to build the swerve drive. Each one has specific fields which correspond to an attribute of the swerve drive, some physical and some abstract.

#### Click on each JSON below to see the configuration options

* `swervedrive.json`
* `controllerproperties.json`
* `pidfpropreties.json`
* `physicalproperties.json`
* `backleft.json`, `backright.json`, `frontleft.json`, `frontright.json`
