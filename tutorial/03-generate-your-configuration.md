# 3. Generate your configuration

Since 2026, the supported way to configure YAGSL is [config.yagsl.com](https://config.yagsl.com), a
web app that builds your configuration files from a guided form instead of you hand-writing JSON.

{% hint style="info" %}
Hand-editing the JSON files is still possible and fully supported — see the
[JSON Schema Reference](../reference/json-schema/) if you need to script config generation or tweak
a value the generator doesn't expose. Most teams should use the generator, though: it validates your
inputs and always produces a schema-correct config.
{% endhint %}

## Fill in the form

The generator is organized into six tabs — work through them using the information you collected in
[step 1](01-gather-your-robot-information.md):

1. **Gyro** — select your gyroscope type and connection (CAN, SPI, etc.), its ID/CAN bus, which axis
   reports yaw, and whether it needs to be inverted.
2. **Front Left**, **Front Right**, **Back Left**, **Back Right** (one tab each) — for every module,
   set the drive motor type/ID, angle motor type/ID, absolute encoder type/ID, any needed inversions,
   and the module's location relative to the robot center.
3. **Properties** — shared physical properties (gearing defaults, current limits) and starting PIDF
   gains for the drive and angle motors.

You can leave absolute encoder offsets and final PIDF gains at their defaults for now — you'll
measure and tune those in the next two steps. The important thing at this stage is getting hardware
types, IDs, and module locations correct.

## Download and unzip

When you're done, download the generated ZIP and unzip it into your project's `src/main/deploy`
directory so the layout looks like this:

```text
src/main/deploy
└── swerve
    └── base
        ├── swervedrive.json
        └── modules
            ├── frontleft.json
            ├── frontright.json
            ├── backleft.json
            ├── backright.json
            ├── physicalproperties.json
            └── pidfproperties.json
```

{% hint style="success" %}
If you already have a config directory from a previous season or a different robot, you can also
upload it back into the generator to review or edit it, instead of starting from scratch.
{% endhint %}

Commit these files to your project's version control along with your code — they're part of your
robot program, not a build artifact.

Next: [verify and calibrate your hardware](04-verify-and-calibrate-hardware.md).
