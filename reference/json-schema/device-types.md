# Device Type Strings

Every device `type` string YAGSL's parser currently recognizes, grouped by device kind. These are
the exact enum values enforced by the JSON Schema files shipped in the vendordep
(`vendordep/src/test/resources/schemas/`) and resolved in code by
`swervelib.parser.deserializer.ReflectionsManager` and the per-vendor classes under
`swervelib.parser.deserializer.reflections`.

{% hint style="info" %}
[config.yagsl.com](https://config.yagsl.com) only lets you pick valid combinations, so you rarely
need to type these strings by hand — this page is for reading/debugging existing config files.
{% endhint %}

## Gyro

Used in [swervedrive.json](swervedrive-json.md)'s `gyro.type`.

| Type string           | Hardware                                    |
| ---------------------- | -------------------------------------------- |
| `navx3_can`            | Studica NavX3, over CAN (CAN FD)             |
| `pigeon2_can`          | CTRE Pigeon 2.0, over CAN                    |
| `canandgyro_can`       | Redux Canandgyro, over CAN                   |
| `systemcore_internal`  | Internal gyro on the SystemCore control system |
| `custom`               | Any gyro not built by the parser (e.g. a roboRIO SPI/I2C Studica AHRS) — you supply it yourself |

See [Gyroscopes](../hardware/gyroscopes.md) for wiring/calibration notes per device.

## Motor Controllers

Used in [module json](module-json.md)'s `drive.type` and `angle.type`. Format is
`<controller>_<motor>`.

| Controller family | Compatible motor suffixes |
| ------------------ | -------------------------- |
| `talonfx_`          | `krakenx44`, `krakenx60` |
| `talonfxs_`         | `neo`, `neo2`, `neo550`, `vortex`, `pulsar`, `minion` |
| `sparkmax_`         | `neo`, `neo2`, `neo550`, `vortex`, `pulsar`, `minion` |
| `sparkflex_`        | `neo`, `neo2`, `neo550`, `vortex`, `pulsar`, `minion` |
| `nova_`             | `neo`, `neo2`, `neo550`, `vortex`, `pulsar`, `minion` |

e.g. `talonfx_krakenx60`, `sparkmax_neo`, `sparkflex_vortex`, `nova_minion`.

{% hint style="warning" %}
The schema doesn't restrict every combination per-controller (e.g. `talonfx_neo` is syntactically
valid JSON) — not every motor is actually usable on every controller family in practice. Stick to
combinations [config.yagsl.com](https://config.yagsl.com) offers, or a combination you've confirmed
via the controller's own vendor tooling.
{% endhint %}

See [Motor Controllers](../hardware/motor-controllers.md) for per-controller notes.

## Absolute Encoders

Used in [module json](module-json.md)'s `absoluteEncoder.type`. Format is roughly
`<encoder>_<connection>`.

| Type string               | Hardware                                                   |
| -------------------------- | ------------------------------------------------------------ |
| `revthroughbore_attached`  | REV Through Bore Encoder, plugged into the motor controller |
| `revthroughbore_dio`       | REV Through Bore Encoder, wired to a roboRIO DIO port        |
| `splineencoder_can`        | Spline encoder, over CAN                                     |
| `cancoder_can`             | CTRE CANcoder, over CAN                                      |
| `canandmag_attached`       | Redux Canandmag, plugged into the motor controller           |
| `canandmag_dio`            | Redux Canandmag, wired to a roboRIO DIO port                 |
| `canandmag_can`            | Redux Canandmag, over CAN                                    |
| `srxmag_attached`          | CTRE Mag Encoder (SRX Mag), plugged into the motor controller|
| `srxmag_analog`            | CTRE Mag Encoder (SRX Mag), wired to a roboRIO analog port    |
| `andymarkhexbore_attached` | AndyMark Hex Bore Encoder, plugged into the motor controller |
| `andymarkhexbore_dio`      | AndyMark Hex Bore Encoder, wired to a roboRIO DIO port       |
| `andymarkhexbore_analog`   | AndyMark Hex Bore Encoder, wired to a roboRIO analog port    |
| `andymarkhexbore_can`      | AndyMark Hex Bore Encoder, over CAN                          |
| `analog5v_attached`        | Generic 5V analog encoder, plugged into the motor controller |
| `analog_attached`          | Generic analog encoder, plugged into the motor controller    |
| `dutycycle_attached`       | Generic duty-cycle encoder, plugged into the motor controller|

See [Absolute Encoders](../hardware/absolute-encoders.md) for per-device notes and calibration tips.
