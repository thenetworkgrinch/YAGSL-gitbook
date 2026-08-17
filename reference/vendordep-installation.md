---
description: Vendor dependencies YAGSL requires and how to install them
---

# Vendordep Installation

## Install YAGSL

YAGSL is listed directly in the **WPILib Vendor Dependencies** panel's catalog (VS Code sidebar) —
open it and click **Install** next to YAGSL, no URL required.

<figure><img src="../assets/modern-yagsl.png" alt=""><figcaption><p>YAGSL listed directly in the WPILib Vendor Dependencies panel.</p></figcaption></figure>

If your WPILib version predates YAGSL being in that catalog, install manually instead: use
**Manage Vendor Libraries → Install new library (online)** and paste:

```
https://yet-another-software-suite.github.io/YAGSL/yagsl/yagsl.json
```

See the [WPILib guide to installing 3rd-party libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#installing-libraries)
for the general workflow.

## Required vendor libraries

YAGSL's own vendordep manifest currently declares these as hard requirements — Gradle will fail to
build without them, even if your robot doesn't use every one of them, because YAGSL is generic
across all supported hardware:

| Vendor | Required by YAGSL's manifest | Needed for |
|---|---|---|
| REVLib | Yes | SparkMAX / SparkFlex |
| CTRE Phoenix 6 | Yes | TalonFX / TalonFXS / Pigeon 2 / CANcoder |
| CTRE Phoenix 5 | Yes | (legacy CTRE device support in the wider WPILib ecosystem) |
| StudicaLib | Yes | NavX3-CAN |
| ThriftyLib | Yes | Nova motor controllers, Thrifty absolute encoders |

{% hint style="warning" %}
If your robot uses a **Canandgyro**, **Canandmag**, or other Redux Robotics device, add
**ReduxLib** yourself — it is not currently in YAGSL's auto-required list, even though the parser
supports Redux devices. Search "ReduxLib" in the Vendordep Tab, or install
`https://frcsdk.reduxrobotics.com/ReduxLib_2026.json` manually.
{% endhint %}

You'll also want the vendor's own tuning/firmware tools installed on your development machine
(not a robot-code dependency, but needed to configure/calibrate hardware):

- [REV Hardware Client 2](https://docs.revrobotics.com/rev-hardware-client-2)
- [CTRE Tuner X](https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html)
- Studica Hardware Manager (NavX3-CAN)

{% hint style="info" %}
Exact vendordep version numbers and URLs change every season and sometimes mid-season during
beta periods — always confirm the current URL against the vendor's own install page or the
WPILib Vendordep Tab search rather than copying an old link from these docs.
{% endhint %}
