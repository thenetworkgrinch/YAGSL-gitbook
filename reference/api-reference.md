# API Reference

YAGSL's own Java surface is intentionally small: `swervelib.parser.SwerveParser` reads your
[JSON config](json-schema/README.md) and hands you back a fully-built **YAMS** `SwerveDrive`. From
that point on, you're driving and configuring a YAMS object, not a YAGSL-specific one — so the
authoritative API reference lives in the YAMS docs, not here.

| Class you get back from `SwerveParser`         | YAMS API reference                                                                                                    |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `yams.mechanisms.swerve.SwerveDrive`              | [SwerveDrive](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-drive)                        |
| `yams.mechanisms.config.SwerveDriveConfig`        | [SwerveDriveConfig](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-drive-config)           |
| `yams.mechanisms.swerve.SwerveModule` (per-module)| [SwerveModule](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-module)                      |
| Module config (built internally from your module JSON) | [SwerveModuleConfig](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-module-config)    |
| `yams.mechanisms.swerve.utility.SwerveInputStream` | [SwerveInputStream](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-input-stream)          |

The full YAMS API reference (all mechanisms, motor controllers, units, and math helpers — not just
swerve) is at [yagsl.gitbook.io/yams](https://yagsl.gitbook.io/yams/api-reference/java-reference).

{% hint style="info" %}
Only `SwerveParser` and the JSON-file classes under `swervelib.parser.json` are YAGSL-specific. If
you find yourself calling a method on `SwerveDrive`, `SwerveModule`, `SwerveDriveConfig`,
`SwerveModuleConfig`, or `SwerveInputStream`, you're looking at YAMS API — go to the links above,
not this repo's source, for the authoritative docs and Javadoc.
{% endhint %}

## Where to look for what

- **"What JSON fields exist?"** → [JSON Schema Reference](json-schema/README.md) (this repo).
- **"What does `SwerveDrive.drive(...)` do, what are all its methods?"** → [YAMS SwerveDrive reference](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-drive).
- **"How do I build a `SwerveDriveConfig` by hand instead of from JSON?"** → [YAMS SwerveDriveConfig reference](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-drive-config).
- **"How do I wire joystick input into field/robot-relative chassis speeds?"** → [YAMS SwerveInputStream reference](https://yagsl.gitbook.io/yams/api-reference/java-reference/swerve/swerve-input-stream).
- **"What changed in the JSON schema recently?"** → [Schema Changes](schema-changes.md).
