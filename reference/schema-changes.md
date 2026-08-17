# Schema Changes (Migrating from pre-2026.8.05)

Between the `2026.4.1` and `2026.8.05` releases, YAGSL was rewritten from its own standalone swerve
implementation into a thin JSON parser that builds a **YAMS** `SwerveDrive`
(`swervelib.parser.SwerveParser` → `yams.mechanisms.swerve.SwerveDrive`). The JSON configuration
schema changed to match what YAMS's config builders need. If your `src/main/deploy/swerve/`
directory predates this, none of your old JSON files will parse correctly against the new schema —
this page is a field-by-field diff to help you port them by hand. For a fresh config, it's easier to
just regenerate everything from [config.yagsl.com](https://config.yagsl.com).

{% hint style="warning" %}
This is a breaking change, not an additive one. The parser silently ignores unknown fields
(`FAIL_ON_UNKNOWN_PROPERTIES = false`), so an old file will "parse" without errors but silently drop
every renamed/restructured field, leaving you with defaults you didn't intend. Don't assume an old
config "still works" just because it loads without an exception — diff it against this page.
{% endhint %}

## `swervedrive.json`

| Old (≤ 2026.4.1)                          | New (≥ 2026.8.05)                        | Notes |
| ------------------------------------------- | ------------------------------------------- | ------- |
| `imu: {type, id, canbus}`                   | `gyro: {type, id, canbus}`                  | Renamed. Old `type` values like `"pigeon2"`, `"pigeon"`, `"navx"` also changed shape — see below. |
| `invertedIMU: boolean`                      | `gyroInvert: boolean`                       | Renamed. |
| *(none)*                                    | `gyroAxis: "yaw"\|"pitch"\|"roll"`          | New, optional, defaults to `"yaw"`. |
| `modules: string[]`                         | `modules: string[]`                         | Unchanged. |

**Old:**
```json
{
  "imu": { "type": "pigeon2", "id": 13, "canbus": "canivore" },
  "invertedIMU": true,
  "modules": ["frontleft.json", "frontright.json", "backleft.json", "backright.json"]
}
```

**New:**
```json
{
  "gyro": { "type": "pigeon2_can", "id": 13, "canbus": "canivore" },
  "gyroAxis": "yaw",
  "gyroInvert": true,
  "modules": ["frontleft.json", "frontright.json", "backleft.json", "backright.json"]
}
```

Note the `type` string itself changed too — old bare names (`"pigeon2"`, `"pigeon"`, `"navx"`) became
explicit `<device>_<connection>` strings (`"pigeon2_can"`). See
[Device Type Strings](json-schema/device-types.md) for the current, complete list.

## Module JSON (e.g. `frontleft.json`)

| Old (≤ 2026.4.1)                              | New (≥ 2026.8.05)                                | Notes |
| ------------------------------------------------ | ---------------------------------------------------- | ------- |
| `encoder: {type, id, canbus}`                     | `absoluteEncoder: {type, id, channel, canbus}`        | Renamed, and gained a `channel` field for analog/DIO-connected encoders. `type` strings changed (`"cancoder"` → `"cancoder_can"`, etc.) — see [Device Type Strings](json-schema/device-types.md). |
| `absoluteEncoderOffset: number`                   | `absoluteEncoderOffset: number`                       | Unchanged (degrees). |
| *(none)*                                          | `absoluteEncoderInverted: boolean`                    | New, optional, defaults to `false`. |
| `drive` / `angle`: `{type, id, canbus}`           | Same shape, but `type` strings changed to `<controller>_<motor>` (e.g. `"sparkmax_neo"` unchanged, but `"talonfx"` alone became `"talonfx_krakenx60"` etc. — the motor is now part of the type string). | See [Device Type Strings](json-schema/device-types.md). |
| *(module-level conversion lived only in `physicalproperties.json`)* | `gearing: {drive: {gearRatio, diameter}, angle: {gearRatio}}` (optional, per-module override) | New: a module can now override the shared gearing without touching `physicalproperties.json`. |
| `inverted: {drive, angle}`                        | `inverted: {drive, angle}`                            | Unchanged. |
| `location: {front, left}`                         | `location: {front, left}`                             | Unchanged (inches). |

**Old:**
```json
{
  "drive": { "type": "sparkmax_neo", "id": 4, "canbus": null },
  "angle": { "type": "sparkmax_neo", "id": 3, "canbus": null },
  "encoder": { "type": "cancoder", "id": 9, "canbus": null },
  "inverted": { "drive": false, "angle": false },
  "absoluteEncoderOffset": -114.609,
  "location": { "front": 12, "left": 12 }
}
```

**New:**
```json
{
  "drive": { "type": "sparkmax_neo", "id": 4, "canbus": "" },
  "angle": { "type": "sparkmax_neo", "id": 3, "canbus": "" },
  "absoluteEncoder": { "type": "cancoder_can", "id": 9, "channel": 0, "canbus": "" },
  "inverted": { "drive": false, "angle": false },
  "absoluteEncoderOffset": -114.609,
  "absoluteEncoderInverted": false,
  "location": { "front": 12, "left": 12 }
}
```

## `modules/physicalproperties.json`

| Old (≤ 2026.4.1)                                | New (≥ 2026.8.05)                                    | Notes |
| --------------------------------------------------- | -------------------------------------------------------- | ------- |
| `conversionFactors: {drive: {gearRatio, diameter, factor}, angle: {gearRatio, factor}}` | `gearing: {drive: {gearRatio, diameter}, angle: {gearRatio}}` | Renamed and simplified — the precomputed `factor` override is gone; YAMS derives the conversion internally from `gearRatio`/`diameter`. |
| `currentLimit: {drive, angle}`                       | `statorCurrentLimit: {drive, angle}`                      | Renamed. Defaults are now `40`/`20` if omitted. |
| `friction: {drive, angle}`                           | *(removed)*                                               | No longer configurable via JSON. |
| `steerRotationalInertia: number`                     | *(removed)*                                               | No longer configurable via JSON. |
| `robotMass: number`                                  | *(removed)*                                               | No longer configurable via JSON. |
| `rampRate: {drive, angle}`                            | *(removed)*                                               | No longer configurable via JSON. |
| `wheelGripCoefficientOfFriction: number`              | *(removed)*                                               | No longer configurable via JSON. |
| `optimalVoltage: number`                              | *(removed)*                                               | No longer configurable via JSON. |

**Old:**
```json
{
  "conversionFactors": {
    "angle": { "gearRatio": 12.8, "factor": 0 },
    "drive": { "gearRatio": 8.14, "diameter": 4, "factor": 0 }
  },
  "currentLimit": { "drive": 40, "angle": 20 },
  "rampRate": { "drive": 0.25, "angle": 0.25 },
  "wheelGripCoefficientOfFriction": 1.19,
  "optimalVoltage": 12
}
```

**New:**
```json
{
  "gearing": {
    "drive": { "gearRatio": 8.14, "diameter": 4.0 },
    "angle": { "gearRatio": 12.8 }
  },
  "statorCurrentLimit": { "drive": 40, "angle": 20 }
}
```

If your old config relied on `rampRate`, `friction`, `robotMass`, `steerRotationalInertia`, or
`wheelGripCoefficientOfFriction` for simulation fidelity, that modeling now lives inside YAMS/the
motor sim itself rather than being team-configurable per robot via this file.

## `modules/pidfproperties.json`

| Old (≤ 2026.4.1)          | New (≥ 2026.8.05)         | Notes |
| ---------------------------- | ---------------------------- | ------- |
| `p`, `i`, `d`                 | `p`, `i`, `d`                 | Unchanged. |
| `f: number`                   | *(removed, replaced by `s`/`v`/`a`)* | Old `f` was a flat feedforward multiplier; the new fields model a proper `SimpleMotorFeedforward` (`kS`, `kV`, `kA`) instead. |
| `iz: number` (integral zone)  | *(removed)*                   | No longer configurable. |
| `output: {min, max}` (clamp)  | *(removed)*                   | No longer configurable via JSON. |
| *(none)*                      | `s`, `v`, `a: number` (all optional) | New feedforward gains. If `drive.v` is left `0`/omitted, the parser auto-derives `kV` from the drive motor's free speed. |

**Old:**
```json
{
  "drive": { "p": 0.00023, "i": 0.0000002, "d": 1, "f": 0, "iz": 0 },
  "angle": { "p": 0.01, "i": 0, "d": 0, "f": 0, "iz": 0 }
}
```

**New:**
```json
{
  "drive": { "p": 0.5, "i": 0.0, "d": 0.01 },
  "angle": { "p": 2.0, "i": 0.0, "d": 0.1 }
}
```

You'll need to re-tune from scratch rather than porting old `p`/`i`/`d` values directly — the old
gains were tuned against a different internal control loop (raw `f`/`iz`) than the new
feedforward-based one. See [How to Tune PIDF Gains](../how-to/tune-pidf-gains.md).

## `controllerproperties.json` — removed entirely

Pre-2026.8.05 YAGSL read a `controllerproperties.json` with an `angleJoystickRadiusDeadband` and a
`heading: {p, i, d}` heading-hold PID, used internally by a `swervelib.SwerveController` class to
build field-relative driving with heading lock. **Both the file and the class are gone.**

```json
// This file no longer does anything — delete it.
{
  "angleJoystickRadiusDeadband": 0.5,
  "heading": { "p": 0.4, "i": 0, "d": 0.01 }
}
```

Field-relative/heading-based driving is now built in your own robot code using YAMS's
`yams.mechanisms.swerve.utility.SwerveInputStream`, not a JSON-configured heading PID. See
[SwerveInputStream reference](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-input-stream)
and the [tutorial's driving step](../tutorial/05-deploy-and-drive.md) for the current pattern.

## Hardware support that was dropped, not just renamed

This isn't just a field-rename — some previously-supported gyro and motor-controller hardware has
**no equivalent type string at all** in the current parser (`swervelib.parser.deserializer.reflections`).
If your old config used one of these, there's no direct migration — you need different hardware or
a custom fork:

- **Gyros:** the roboRIO-attached NavX2 (`navx`, `navx_spi`, `navx_i2c`, `navx_mxp_serial`,
  `navx_usb`), the original Pigeon (gen 1), and analog SPI gyros (`adxrs450`, `adis16448`,
  `adis16470`) are all gone. Only `pigeon2_can`, `navx3_can`, and `canandgyro_can` are currently
  supported. (`systemcore_internal` is listed in the schema but currently throws
  `"Internal gyro not supported yet!"` — don't use it yet.)
- **Motor controllers:** `talonsrx` and brushed `sparkmax_brushed` are gone — only the five
  brushless controller families (`talonfx`, `talonfxs`, `sparkmax`, `sparkflex`, `nova`) are
  supported now, each paired with a brushless motor. See
  [Device Type Strings](json-schema/device-types.md).

If you're on old gyro/motor hardware that's no longer supported, budget time to swap hardware, not
just JSON — this is a real capability change, not a documentation gap.

## Behavior changes that aren't JSON fields

- **Cosine compensation** used to be toggleable per-module (`useCosineCompensator`, default `true`).
  It's now always applied by the parser — there's no way to disable it via config.
- **Deploy directory construction** changed from directly instantiating `SwerveDrive`/module
  classes to `new SwerveParser(dir).createSwerveDrive(new SwerveDriveConfig()...)`. See
  [Deploy and Drive](../tutorial/05-deploy-and-drive.md).

## Migration checklist

1. Regenerate from [config.yagsl.com](https://config.yagsl.com) if at all possible — it's faster and
   less error-prone than hand-porting every field above.
2. If hand-porting: rename `imu`→`gyro`, `invertedIMU`→`gyroInvert` in `swervedrive.json`; add
   `gyroAxis: "yaw"`.
3. Rename `encoder`→`absoluteEncoder` in every module file, add a `channel` (usually `0` if CAN),
   add `absoluteEncoderInverted: false`.
4. Update every `type` string to the new `<device>_<connection>` convention — see
   [Device Type Strings](json-schema/device-types.md).
5. Replace `conversionFactors`→`gearing` in `physicalproperties.json`, dropping any `factor`
   override. Replace `currentLimit`→`statorCurrentLimit`.
6. Delete `controllerproperties.json`; move any heading-lock behavior into robot code via
   `SwerveInputStream`.
7. Re-tune PIDF from scratch using the new `p`/`i`/`d`/`s`/`v`/`a` shape — old gains don't transfer.
8. Delete any `rampRate`/`friction`/`robotMass`/`steerRotationalInertia`/
   `wheelGripCoefficientOfFriction`/`optimalVoltage` fields — they're ignored now.
