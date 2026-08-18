---
description: Supported motor controller/motor combinations for module drive and angle motors
---

# Motor Controllers

{% hint style="danger" %}
**TalonSRX and brushed SparkMAX are no longer supported.** The pre-2026.8.05 `talonsrx` and
`sparkmax_brushed` types have been removed — YAGSL now only drives brushless motors through
YAMS's `SmartMotorController` abstraction. See [Schema Changes](../schema-changes.md).
{% endhint %}

## Motor Checklist

* [ ] All motors spin counterclockwise positive (CCW+); document any inversions required to
  achieve that.
* [ ] Motor is updated to the latest firmware.
* [ ] Motor has a unique CAN ID that matches its module JSON.
* [ ] Wheels are aligned forwards with all bevels facing the same way before first enable.
* [ ] PID is tuned — see [Tune PIDF Gains](../../how-to/tune-pidf-gains.md).

## Configuring a motor

Each module JSON (`frontleft.json`, etc.) has a `drive` and an `angle` motor, both `DeviceJson`
objects of the form `"<controller>_<motor>"`:

```json
{
  "drive": { "type": "sparkflex_vortex", "id": 5, "canbus": "" },
  "angle":  { "type": "sparkmax_neo550", "id": 6, "canbus": "" }
}
```

The `<motor>` suffix tells YAGSL which `DCMotor` model to use for feedforward and simulation
characterization (free speed, stall torque, etc.) — it must match the physical motor attached to
that controller.

## Supported controller × motor combinations

| Controller | `type` values |
|---|---|
| [TalonFX](#talonfx-krakenx60-krakenx44) | `talonfx_krakenx44`, `talonfx_krakenx60` |
| [TalonFXS](#talonfxs) | `talonfxs_neo`, `talonfxs_neo2`, `talonfxs_neo550`, `talonfxs_vortex`, `talonfxs_pulsar`, `talonfxs_minion` |
| [SparkMAX](#sparkmax) | `sparkmax_neo`, `sparkmax_neo2`, `sparkmax_neo550`, `sparkmax_vortex`, `sparkmax_pulsar`, `sparkmax_minion` |
| [SparkFlex](#sparkflex) | `sparkflex_neo`, `sparkflex_neo2`, `sparkflex_neo550`, `sparkflex_vortex`, `sparkflex_pulsar`, `sparkflex_minion` |
| Nova (ThriftyBot) | `nova_neo`, `nova_neo2`, `nova_neo550`, `nova_vortex`, `nova_pulsar`, `nova_minion` |

{% hint style="warning" %}
This is the full cross-product the config generator and parser accept for feedforward/simulation
purposes — it is **not** a claim that every combination is physically wireable. Respect real
hardware constraints: TalonFX only physically drives Kraken X44/X60 motors, SparkMAX/SparkFlex
drive REV motors via their own connectors, and Nova drives ThriftyBot motors. Pick the row for
your actual controller, and the `<motor>` suffix that matches what's physically bolted to it.
{% endhint %}

Only CTRE controllers (`talonfx_*`, `talonfxs_*`) support the `canbus` field for CANivore use — set
it to `""` for your control system's default bus (`rio` on a roboRIO). See
[Control system and CAN buses](../json-schema/README.md#control-system-and-can-buses).

## TalonFX (Kraken X60 / Kraken X44)

TalonFX is the controller for Kraken X44/X60 and (legacy) Falcon 500 motors. Commonly used as a
drive motor for its power and efficiency versus REV NEOs.

- [Kraken product page](https://wcproducts.com/products/kraken)
- Upgradeable/configurable via [Tuner X](https://v6.docs.ctr-electronics.com/en/latest/docs/tuner/index.html) —
  watch the LED status code when debugging. Settings changed in Tuner X are overwritten on startup
  by YAGSL.

{% hint style="warning" %}
If running Falcon 500 v2 (effectively discontinued), apply the
[Loctite fix](https://content.vexrobotics.com/vexpro/Falcon/217-6515-753-Falcon500-V2-Upgrade.pdf).
{% endhint %}

## TalonFXS

TalonFXS is CTRE's controller for third-party/non-Kraken brushless motors (NEO, NEO Vortex,
Minion, Pulsar) wired through CTRE's ecosystem — same Tuner X tooling and CAN/CANivore support as
TalonFX.

## SparkMAX

REV SparkMAX, brushless mode (NEO / NEO 550 / NEO Vortex / Minion / Pulsar via the appropriate
`<motor>` suffix).

- [Product page](https://www.revrobotics.com/rev-11-2158/)
- Requires the [REV Hardware Client](https://docs.revrobotics.com/rev-hardware-client/gs/install)
  to test PID, run at a set percentage, and update firmware.

{% hint style="warning" %}
The SparkMAX must be disconnected from the CAN bus **on startup** to be recognized by the REV
Hardware Client.
{% endhint %}

To tune PID directly on the controller: open REV Hardware Client → select the SparkMAX → Telemetry
tab → set to position control while adjusting gains in the left pane.

**Checklist**

* [ ] Unique CAN ID.
* [ ] Latest firmware.
* [ ] Rotates CCW+ (invert programmatically if not).
* [ ] PID tuned for one drive motor and one angle motor before wiring the rest.

## SparkFlex

REV SparkFlex, brushless mode (same motor suffixes as SparkMAX, primarily used with NEO Vortex).

- [Product page](https://www.revrobotics.com/next-generation-spark-neo/)
- Same [REV Hardware Client](https://docs.revrobotics.com/rev-hardware-client-2) workflow and
  startup-disconnect caveat as SparkMAX.

---

### General motor tips (apply regardless of controller)

- Confirm CCW+ rotation on the bench before mounting — see [When to Invert](../../how-to/determine-inversion.md).
- Tune one drive motor and one angle motor fully before copying gains to the other three modules;
  see [Tune PIDF Gains](../../how-to/tune-pidf-gains.md).
- Current limits and gearing live in `physicalproperties.json` (or a per-module `gearing`
  override) — see [physicalproperties.json Reference](../json-schema/physicalproperties-json.md).
