# Physical Properties Configuration

## Swerve Module Physical Properties (`modules/physicalproperties.json`)

This JSON configures the physical properties shared with all the Swerve Modules. It maps 1:1 with [`PhysicalPropertiesJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/PhysicalPropertiesJson.html) which creates [`SwerveModulePhysicalCharacteristics`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModulePhysicalCharacteristics.html).

{% hint style="info" %}
Use the [standard-conversion-factors.md](../standard-conversion-factors.md "mention") page to set the conversion factors for your robot if you're using a COTS swerve module!
{% endhint %}

## Fields

<table data-full-width="true"><thead><tr><th>Name</th><th>Units</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>conversionFactor</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a> calculated using the formula <a href="../../fundamentals/swerve-modules.md#conversion-factor">here</a>.</td><td>N</td><td>Conversion factor applied to the motor controller for the onboard PID.</td></tr><tr><td><code>optimalVoltage</code></td><td>Voltage</td><td>Y</td><td>Optimal voltage to compensate to and base feedforward calculations off of.</td></tr><tr><td><code>currentLimit</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a> <br>More info <a href="../../fundamentals/swerve-modules.md#current-limiting">here</a>.</td><td>N</td><td>The current limit in AMPs to apply to the motors.</td></tr><tr><td><code>rampRate</code></td><td><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a></td><td>N</td><td>The minimum number of seconds to take for the motor to go from 0 to full throttle.</td></tr><tr><td><code>wheelGripCoefficientOfFriction</code></td><td>Coefficient of Friction on Carpet</td><td>N</td><td>The grip tape coefficient of friction on carpet. Used to calculate the practical maximum acceleration.</td></tr></tbody></table>

#### MotorConfig

| Name    | Units  | Required | Description        |
| ------- | ------ | -------- | ------------------ |
| `drive` | Number | Y        | Drive motor value. |
| `angle` | Number | Y        | Angle motor value. |

## Example Configuration File

<pre class="language-json"><code class="lang-json">{
  <a data-footnote-ref href="#user-content-fn-1">"conversionFactor"</a>: {
	"angle": 28.125,
	"drive": 0.047286787200699704
  },
  "currentLimit": {
    "drive": 40,
    "angle": 20
  },
  "rampRate": {
    "drive": 0.25,
    "angle": 0.25
  },
  "wheelGripCoefficientOfFriction": 1.19,
  "optimalVoltage": 12
}
</code></pre>

[^1]: Can be found for COTS swerve modules in [standard-conversion-factors.md](../standard-conversion-factors.md "mention")
