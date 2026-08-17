# JSON Configuration Schema

YAGSL builds a YAMS `SwerveDrive` from a directory of JSON files. `swervelib.parser.SwerveParser`
reads this directory and constructs the equivalent YAMS `SwerveDriveConfig` /
`SwerveModuleConfig` / `SmartMotorControllerConfig` objects for you — you rarely need to hand-edit
these files, since [config.yagsl.com](https://config.yagsl.com) generates them for you, but knowing
the schema is useful for debugging, scripting, or hand-tweaking a single value.

{% hint style="info" %}
If you're setting up a robot for the first time, use the [Tutorial](../../tutorial/README.md)
section instead — this is reference material for looking up exact field names and types.
{% endhint %}

## Directory layout

```
src/main/deploy
└── swerve
    └── base                       <- name is arbitrary, passed to `new SwerveParser(...)`
        ├── swervedrive.json
        └── modules
            ├── frontleft.json
            ├── frontright.json
            ├── backleft.json
            ├── backright.json     <- module filenames are arbitrary, referenced by swervedrive.json
            ├── physicalproperties.json
            ├── pidfproperties.json
            └── pidfproperties_sim.json   (optional)
```

`SwerveParser` reads, in order:

1. `swervedrive.json` — top-level drive config: gyro + list of module files.
2. `modules/physicalproperties.json` — default gearing and current limits shared by every module.
3. `modules/pidfproperties.json` — default PID + feedforward gains shared by every module. If
   `modules/pidfproperties_sim.json` also exists, it's used instead whenever
   `RobotBase.isSimulation()` is `true`, so you can tune sim and real gains independently.
4. Each file listed in `swervedrive.json`'s `modules` array — one per swerve module, describing that
   module's hardware, wiring, and location. A module file may override the gearing from
   `physicalproperties.json` with its own `gearing` object.

All JSON is parsed with Jackson in lenient mode (`FAIL_ON_UNKNOWN_PROPERTIES = false`), so unknown
fields are ignored rather than rejected — useful if you're migrating a file field-by-field.

## Pages in this section

- [swervedrive.json](swervedrive-json.md)
- [module json (per-module file)](module-json.md)
- [physicalproperties.json](physicalproperties-json.md)
- [pidfproperties.json / pidfproperties_sim.json](pidfproperties-json.md)
- [Device type strings](device-types.md) — every valid `type` value for gyros, motor controllers,
  and absolute encoders.

{% hint style="warning" %}
Coming from a pre-2026.8.05 `swerve/` directory (`imu`, `encoder`, `conversionFactors`,
`controllerproperties.json`, PIDF `f`/`iz`)? See [Schema Changes](../schema-changes.md) for a full
migration guide.
{% endhint %}
