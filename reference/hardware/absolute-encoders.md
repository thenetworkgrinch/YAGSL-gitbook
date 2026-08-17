---
description: Supported absolute encoder types for a module's absoluteEncoder field
---

# Absolute Encoders

YAGSL supports most common FRC absolute encoders. An absolute encoder is required for every
module — it's what lets the angle motor know its true position on power-up, without needing to
"home" against a hard stop.

The live reading shows up on the dashboard under `swerve/modules/.../Raw Absolute Encoder` — use
it to find the value for `absoluteEncoderOffset` in each module's JSON.

## Absolute Encoder Checklist

* [ ] All absolute encoders read counterclockwise positive (CCW+).
* [ ] Magnetic absolute encoders get a good read on the magnet both at rest and while the robot is
  moving (check for wiggle/wire strain).
* [ ] Each encoder has a unique CAN ID or analog/DIO channel.
* [ ] Encoder reads the full `0°`–`360°` range.

## Configuring an absolute encoder

`absoluteEncoder` is a `DeviceJson`-shaped object, `"<vendor>_<connection>"`:

```json
{
  "absoluteEncoder": { "type": "cancoder_can", "id": 11, "channel": 0, "canbus": "" },
  "absoluteEncoderOffset": -18.281,
  "absoluteEncoderInverted": false
}
```

- `absoluteEncoderOffset` — degrees to add so the wheel reads `0°` when facing forward with the
  bevel gear to the left. Read this off `Raw Absolute Encoder` on the dashboard with the wheel
  manually squared up, then enter the negative of that raw value (config.yagsl.com does this
  arithmetic for you).
- `absoluteEncoderInverted` — should rarely, if ever, need to be `true`.
- For encoders wired to an analog or DIO channel, set `channel` instead of `id`/`canbus`.

{% hint style="warning" %}
If a module spins continuously without settling, try inverting its angle motor before suspecting
the absolute encoder.
{% endhint %}

## Supported absolute encoder types

| Device | `type` | Connection |
|---|---|---|
| [REV Throughbore](https://www.revrobotics.com/rev-11-1271/) | `revthroughbore_attached` | Plugged into the angle motor controller's duty-cycle port |
| REV Throughbore | `revthroughbore_dio` | roboRIO DIO |
| CTRE [CANcoder](https://pro.docs.ctr-electronics.com/en/latest/docs/hardware-reference/cancoder/index.html) | `cancoder_can` | CAN; supports CANivore |
| [Canandmag](https://docs.reduxrobotics.com/canandmag/getting-started) | `canandmag_attached` | Plugged into the angle motor controller's duty-cycle port |
| Canandmag | `canandmag_dio` | roboRIO DIO |
| Canandmag | `canandmag_can` | CAN |
| [SRX Mag Encoder](https://store.ctr-electronics.com/srx-mag-encoder/) | `srxmag_attached` | Plugged into the angle motor controller's duty-cycle port |
| SRX Mag Encoder | `srxmag_analog` | roboRIO analog input |
| [AndyMark Hex Bore](https://www.andymark.com/) | `andymarkhexbore_attached` | Plugged into the angle motor controller's duty-cycle port |
| AndyMark Hex Bore | `andymarkhexbore_dio` | roboRIO DIO |
| AndyMark Hex Bore | `andymarkhexbore_analog` | roboRIO analog input |
| AndyMark Hex Bore | `andymarkhexbore_can` | CAN |
| Spline Encoder | `splineencoder_can` | CAN (REV) |
| Generic analog (5V) | `analog5v_attached` | Plugged into the angle motor controller's 5V analog port |
| Generic analog | `analog_attached` | Plugged into the angle motor controller's analog port |
| Generic duty-cycle | `dutycycle_attached` | Plugged into the angle motor controller's duty-cycle port |

The `_attached` connection means the encoder is wired directly into the angle motor controller
(SparkMAX/SparkFlex duty-cycle or analog port, etc.) rather than a roboRIO or CAN device — YAGSL
reads it back through that controller's vendor API. This replaces the old
`SwerveDrive.pushOffsetsToEncoders()` mechanism from pre-2026.8.05 releases.

---

### General absolute encoder tips (apply regardless of device)

- Set the offset with the wheel physically squared to the robot, not by eyeballing degrees on the
  dashboard from an arbitrary position.
- Re-check offsets any time a module is disassembled — even a "put it back the same way" rebuild
  can shift the magnet by a few degrees, and swerve is unforgiving of small offset errors at low
  speed.
- If you swap a module between corners, its encoder wiring moves with it — the offset value does
  not automatically follow. See [Verify Module Locations](../../how-to/verify-module-locations.md).
