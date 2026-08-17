---
description: Supported gyroscope/IMU types for swervedrive.json's gyro field
---

# Gyroscopes

{% hint style="danger" %}
**Hardware support narrowed in the 2026.8.05 rewrite.** YAGSL now only instantiates gyros over
**CAN**. The roboRIO SPI/I2C/USB-serial NavX (original NavX2 `navx`/`navx_spi`/`navx_i2c`/
`navx_mxp_serial`/`navx_usb`), the original CTRE **Pigeon** (gen 1), and the analog SPI gyros
(**ADXRS450**, **ADIS16448**, **ADIS16470**) are **not supported** by the current parser. If your
robot uses one of those devices, see [Schema Changes](../schema-changes.md) — you'll need to
either move to a supported CAN gyro or stay on a pre-2026.8.05 YAGSL release.
{% endhint %}

## Gyroscope Checklist

* [ ] Gyroscope readings increase when rotated counter-clockwise (CCW+).
* [ ] Yaw reading is the robot heading.
* [ ] Gyroscope `0°` is the desired robot "front".

## Configuring the gyro

In `swervedrive.json` the gyro is one object plus two drive-wide settings:

```json
{
  "gyro": {
    "type": "pigeon2_can",
    "id": 13,
    "canbus": "canivore"
  },
  "gyroAxis": "yaw",
  "gyroInvert": true,
  "modules": ["frontleft.json", "frontright.json", "backleft.json", "backright.json"]
}
```

- `gyro.type` — one of the supported types below, formatted `vendor_connection`.
- `gyro.id` — CAN ID of the device (ignored where not applicable).
- `gyro.canbus` — CAN bus name. Use `""` for the roboRIO bus, or a CANivore name if the device is
  on one.
- `gyroAxis` — which physical axis of the sensor to read as robot heading: `yaw` (default),
  `pitch`, or `roll`. Only change this if the sensor is mounted on an edge or angle.
- `gyroInvert` — invert the heading reading. If your robot spins out of control with no controller
  input, invert this.

{% hint style="warning" %}
Only CTRE devices support the `canbus` option. If your device is on the roboRIO CAN bus, use
`""`. If it's on a CANivore, `canbus` must match the CANivore's configured name.
{% endhint %}

## Supported gyroscope types

| Device | `type` | Communication |
|---|---|---|
| [Pigeon 2](#pigeon-2) | `pigeon2_can` | CAN; supports CANivore |
| [Canandgyro](https://docs.reduxrobotics.com/canandgyro/getting-started) | `canandgyro_can` | CAN; roboRIO bus only |
| [NavX3-CAN](#navx3-can) | `navx3_can` | CAN 2.0 / CAN FD |
| SystemCore internal IMU | `systemcore_internal` | *listed in the config schema but not yet implemented — the parser throws if selected. Do not use yet.* |

If you need a gyro not on this list, an analog gyro on the roboRIO can still be wired up outside
the JSON config and read manually — YAGSL's JSON parser itself only builds the types above.

## NavX3-CAN

{% hint style="info" %}
NavX3-CAN supports CAN 2.0 (roboRIO) and has CAN-FD capability. See Studica's notes for CAN-FD
requirements.
{% endhint %}

- [Studica product page](https://www.studica.co/navx3-can-imu)
- [Studica NavX releases, firmware tools, and vendordeps](https://github.com/Studica-Robotics/NavX)

**Checklist**

* [ ] Install the **StudicaLib** vendordep (do not install both `Studica` and `StudicaLib`).
* [ ] Update NavX3-CAN firmware to **5.0.4+** using Studica Hardware Manager.
* [ ] Use Studica Hardware Manager to find the CAN ID (often ships as `0`) and enter it in your
  config.

NavX3-CAN ships factory-calibrated. For higher accuracy, re-calibrate with Studica Hardware
Manager — it auto-detects sensor orientation during calibration. Connect CAN-H/CAN-L to the robot
bus and make sure the bus is properly terminated.

## Pigeon 2

- [Product page](https://store.ctr-electronics.com/pigeon-2/)
- [Hardware reference](https://pro.docs.ctr-electronics.com/en/latest/docs/hardware-reference/pigeon2/index.html)
- Upgradeable via [Tuner X](https://v6.docs.ctr-electronics.com/en/latest/docs/tuner/index.html) —
  pay attention to the LED status code when debugging. Any settings changed in Tuner X are
  overwritten on startup by YAGSL.

**Checklist**

* [ ] Yaw increments counter-clockwise positive.
* [ ] Pigeon 2 is mounted as close to the robot's center as possible.
* [ ] Updated to the latest firmware, on a unique CAN ID.
* [ ] Calibrated once installed on the robot.

Communicates over CAN and can be paired with a [CANivore](https://store.ctr-electronics.com/canivore/)
to keep it off the roboRIO's CAN bus — [set a CANivore name](https://pro.docs.ctr-electronics.com/en/latest/docs/canivore/canivore-setup.html)
and use it as `canbus`.

## Canandgyro

- [Getting started](https://docs.reduxrobotics.com/canandgyro/getting-started)
- CAN only; does not support CANivore.

---

### General gyro tips (still apply regardless of device)

- Mount the gyro as close to the robot's center of rotation as practical — off-center mounting
  couples translational vibration into the heading reading.
- Always keep the robot still for a few seconds after power-on if your device does an on-the-fly
  calibration; moving during that window produces a bad heading for the rest of the match.
- If the robot's heading drifts over a match, see [Diagnose Swerve Drift](../../how-to/diagnose-swerve-drift.md).
