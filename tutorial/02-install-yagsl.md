# 2. Install YAGSL

## Add the YAGSL vendordep

Installing YAGSL no longer requires typing a URL. Open the **WPILib Vendor Dependencies** panel in
the VS Code sidebar (the WPILib extension icon) — YAGSL is listed directly in the catalog alongside
the other common FRC vendor libraries (PathplannerLib, photonlib, REVLib, Studica, URCL, etc.).
Click **Install** next to it.

<figure><img src="../assets/modern-yagsl.png" alt=""><figcaption><p>YAGSL listed directly in the WPILib Vendor Dependencies panel.</p></figcaption></figure>

If your WPILib version doesn't have YAGSL in that catalog yet, install it the manual way instead:
open **Manage Vendor Libraries → Install new library (online)** and paste the URL:

```
https://yet-another-software-suite.github.io/YAGSL/yagsl/yagsl.json
```

Run a Gradle build afterward to pull down the library before continuing.

## Add vendordeps for your hardware

YAGSL wraps [YAMS](https://yams.yamgen.com/) and talks to hardware through each vendor's own library.
Install a vendordep for every piece of hardware your robot actually uses — same panel as YAGSL, same
one-click **Install**:

| Vendor | Needed for |
|---|---|
| CTRE Phoenix 6 | TalonFX, TalonFXS, Pigeon 2.0, CANcoder |
| CTRE Phoenix 5 | TalonSRX |
| REVLib | SparkMAX, SparkFlex |
| Studica | NavX / NavX3 |
| ReduxLib | Canandgyro, Canandmag, Canandcoder |
| maple-sim | Physics simulation (optional, recommended) |

{% hint style="info" %}
If a vendor isn't in the panel's catalog yet on your WPILib version, install it the manual way
instead — [installing 3rd-party libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#installing-libraries)
covers pasting a URL, which you can get from the vendor's own install page.
{% endhint %}

{% hint style="warning" %}
Your vendor's hardware client (REV Hardware Client, Phoenix Tuner X) is a separate tool from the
vendordep — install it too, it's needed for firmware updates and low-level device configuration.
{% endhint %}

Once your build succeeds with all vendordeps installed, move on to
[generating your configuration](03-generate-your-configuration.md).
