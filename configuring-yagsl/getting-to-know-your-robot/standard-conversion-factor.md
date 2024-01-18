# Standard Conversion Factor

This page serves a standard set of conversion factors for a few typical modules.

{% hint style="warning" %}
IF you are using absolute encoders attached to your SparkMax the `angle` conversion factor will be `360`!
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
    System.out.println("\"conversionFactor\": {");
    System.out.println("\t\"angle\": " + angleConversionFactor + ",");
    System.out.println("\t\"drive\": " + driveConversionFactor);
    System.out.println("}");
```

## MAX Swerve

```json
"conversionFactor": {
    "angle": 360,
    "drive": 0.047286787200699704
  }
```

## Swerve Drive Specialties (SDS)

{% hint style="warning" %}
These conversion factors assume you are using 4in wheels!&#x20;
{% endhint %}

{% tabs %}
{% tab title="MK4i L1 " %}
```json
"conversionFactor": {
	"angle": 16.8,
	"drive": 0.03921201641335663
}
```
{% endtab %}

{% tab title="MK4i L2" %}
```java
"conversionFactor": {
	"angle": 16.8,
	"drive": 0.047286787200699704
}
```
{% endtab %}

{% tab title="MK4i L3" %}
```java
"conversionFactor": {
	"angle": 16.8,
	"drive": 0.05215454470665408
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="MK4 L1" %}
```json
"conversionFactor": {
	"angle": 28.125,
	"drive": 0.03921201641335663
}
```
{% endtab %}

{% tab title="MK4 L2" %}
```json
"conversionFactor": {
	"angle": 28.125,
	"drive": 0.047286787200699704
}
```
{% endtab %}

{% tab title="MK4 L3" %}
```json
"conversionFactor": {
	"angle": 28.125,
	"drive": 0.05215454470665408
}
```
{% endtab %}

{% tab title="MK4 L4" %}
```json
"conversionFactor": {
	"angle": 28.125,
	"drive": 0.06209840731609397
}
```
{% endtab %}
{% endtabs %}
