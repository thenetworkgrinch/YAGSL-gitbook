# When to invert?

Swerve Modules and Swerve Drives require some inversions to get working properly. The goal is to get everything to increase counter clockwise positive!

{% hint style="warning" %}
When your gears are grinding on the ground but not while on blocks and your wheels are facing and spinning in the right directions you may need to tune PID instead of inverting!
{% endhint %}

{% hint style="warning" %}
IF you are inverted incorrectly your modules may spin "out-of-control"
{% endhint %}

## Swerve Motors

When you spin your motor counterclockwise the value in Shuffleboard/NetworkTables `Module[...] Raw Absolute Encoder` and `Module[...] Raw Relative Encoder` should both increase.

### If the `Module[...] Raw Angle Encoder` is decreasing...

Invert your angle motor for every module that is decreasing!

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
    "drive": false,
<strong>    "angle": true
</strong>  },
  "absoluteEncoderInverted": false,
  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

### If the `Module[...] Raw Absolute Encoder` is decreasing...

Invert the absolute encoder in the module JSON with `absoluteEncoderInverted` as shown here.

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
    "drive": false,
    "angle": false
  },
<strong>  "absoluteEncoderInverted": true,
</strong>  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>

### If the `Module[...] Raw Drive Encoder` is decreasing...

Invert your drive motor for every module that is decreasing!

<pre class="language-json"><code class="lang-json">{
  "drive": {
    "type": "sparkmax",
    "id": 2,
    "canbus": null
  },
  "angle": {
    "type": "sparkmax",
    "id": 1,
    "canbus": null
  },
  "encoder": {
    "type": "cancoder",
    "id": 10,
    "canbus": null
  },
  "inverted": {
<strong>    "drive": true,
</strong>    "angle": false
  },
  "absoluteEncoderInverted": false,
  "absoluteEncoderOffset": -50.977,
  "location": {
    "front": 12,
    "left": -12
  }
}
</code></pre>
