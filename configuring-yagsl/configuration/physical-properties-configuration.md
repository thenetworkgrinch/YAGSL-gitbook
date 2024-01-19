# Physical Properties Configuration

## Swerve Module Physical Properties (`physicalproperties.json`)

This JSON configures the physical properties shared with all the Swerve Modules. It maps 1:1 with [`PhysicalPropertiesJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/PhysicalPropertiesJson.html) which creates [`SwerveModulePhysicalCharacteristics`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveModulePhysicalCharacteristics.html).

{% hint style="info" %}
Use the [standard-conversion-factors.md](../standard-conversion-factors.md "mention") page to set the conversion factors for your robot if you're using a COTS swerve module!
{% endhint %}

## Fields

| Name                             | Units                                                                                                                                                                     | Required | Description                                                                                            |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| `conversionFactor`               | [MotorConfig](physical-properties-configuration.md#motorconfig) calculated using the formula [here](../../fundamentals/swerve-modules.md#conversion-factor).              | N        | Conversion factor applied to the motor controller for the onboard PID.                                 |
| `optimalVoltage`                 | Voltage                                                                                                                                                                   | Y        | Optimal voltage to compensate to and base feedforward calculations off of.                             |
| `currentLimit`                   | <p><a href="physical-properties-configuration.md#motorconfig">MotorConfig</a> <br>More info <a href="../../fundamentals/swerve-modules.md#current-limiting">here</a>.</p> | N        | The current limit in AMPs to apply to the motors.                                                      |
| `rampRate`                       | [MotorConfig](physical-properties-configuration.md#motorconfig)                                                                                                           | N        | The minimum number of seconds to take for the motor to go from 0 to full throttle.                     |
| `wheelGripCoefficientOfFriction` | Coefficient of Friction on Carpet                                                                                                                                         | N        | The grip tape coefficient of friction on carpet. Used to calculate the practical maximum acceleration. |

#### MotorConfig

| Name    | Units  | Required | Description        |
| ------- | ------ | -------- | ------------------ |
| `drive` | Number | Y        | Drive motor value. |
| `angle` | Number | Y        | Angle motor value. |

