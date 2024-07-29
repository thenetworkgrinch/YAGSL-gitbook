# Standard Conversion Factors

This page serves a standard set of conversion factors for a few typical modules.

{% hint style="warning" %}
IF you are using absolute encoders attached to your SparkMAX data port (on the top of the SparkMAX) the `angle` conversion factor should be set to `360`!

For example an MK4i L1 with an absolute encoder attached to the SparkMAX (like a canandcoder) should be set with these conversion factors.

```json
"conversionFactors": {
	"angle": {"factor": 360},
	"drive": {"factor": 0.03921201641335663}
}
```
{% endhint %}

## How I found these?

```java
    // Angle conversion factor is 360 / (GEAR RATIO)
    //  In this case the gear ratio is 12.8 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double angleConversionFactor = SwerveMath.calculateDegreesPerSteeringRotation(12.8);
    // Motor conversion factor is (PI * WHEEL DIAMETER IN METERS) / (GEAR RATIO).
    //  In this case the wheel diameter is 4 inches, which must be converted to meters to get meters/second.
    //  The gear ratio is 6.75 motor revolutions per wheel rotation.
    //  The encoder resolution per motor revolution is 1 per motor revolution.
    double driveConversionFactor = SwerveMath.calculateMetersPerRotation(Units.inchesToMeters(4), 6.75);
    System.out.println("\"conversionFactors\": {");
    System.out.println("\t\"angle\": {\"factor\": " + angleConversionFactor + "},");
    System.out.println("\t\"drive\": {\"factor\": " + driveConversionFactor + "}");
    System.out.println("}");
```

## MAX Swerve

{% hint style="warning" %}
These conversion factors assume you are using 3in wheels!
{% endhint %}

{% tabs %}
{% tab title="12T" %}
```json
"conversionFactors": {
    "angle": {"factor": 360},
    "drive": {"factor": 0.04352533821882586}
}
```
{% endtab %}

{% tab title="13T" %}
```json
"conversionFactors": {
    "angle": {"factor": 360},
    "drive": {"factor": 0.04712388980384689}
}
```
{% endtab %}

{% tab title="14T" %}
```json
"conversionFactors": {
    "angle": {"factor": 360},
    "drive": {"factor": 0.05082576649756735}
}
```
{% endtab %}
{% endtabs %}

## Swerve Drive Specialties (SDS)

{% hint style="warning" %}
These conversion factors assume you are using 4in wheels!&#x20;
{% endhint %}

{% tabs %}
{% tab title="MK4i L1 " %}
```json
"conversionFactors": {
	"angle": {"factor": 16.8},
	"drive": {"factor": 0.03921201641335663}
}
```
{% endtab %}

{% tab title="MK4i L2" %}
```json
"conversionFactors": {
	"angle": {"factor": 16.8},
	"drive": {"factor": 0.047286787200699704}
}
```
{% endtab %}

{% tab title="MK4i L3" %}
```json
"conversionFactors": {
	"angle": {"factor": 16.8},
	"drive": {"factor": 0.05215454470665408}
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK4 L1" %}
```json
"conversionFactors": {
	"angle": {"factor": 28.125},
	"drive": {"factor": 0.03921201641335663}
}
```
{% endtab %}

{% tab title="MK4 L2" %}
```json
"conversionFactors": {
	"angle": {"factor": 28.125},
	"drive": {"factor": 0.047286787200699704}
}
```
{% endtab %}

{% tab title="MK4 L3" %}
```json
"conversionFactors": {
	"angle": {"factor": 28.125},
	"drive": {"factor": 0.05215454470665408}
}
```
{% endtab %}

{% tab title="MK4 L4" %}
```json
"conversionFactors": {
	"angle": {"factor": 28.125},
	"drive": {"factor": 0.06209840731609397}
}
```
{% endtab %}
{% endtabs %}
