# Swerve Drive Configuration

## Swerve Drive (`swervedrive.json`)

The Swerve Drive JSON configuration file configures everything related to the overall Swerve Drive. It maps 1:1 to [`SwerveDriveJson`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/json/SwerveDriveJson.html) which creates a [`SwerveDriveConfiguration`](https://broncbotz3481.github.io/YAGSL/swervelib/parser/SwerveDriveConfiguration.html) that is used to create the [`SwerveDrive`](https://broncbotz3481.github.io/YAGSL/swervelib/SwerveDrive.html) object.

## JSON Fields

<table data-full-width="true"><thead><tr><th>Name</th><th>Units</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>imu</code></td><td><a href="../../devices/gyroscope.md#gyroscope-configuration">Gyroscope</a></td><td>Y</td><td>Robot IMU used to determine heading of the robot.</td></tr><tr><td><code>invertedIMU</code></td><td>Boolean</td><td>Y</td><td>Inversion state of the IMU.</td></tr><tr><td><code>modules</code></td><td>String array</td><td>Y</td><td>Module JSONs in order clockwise order starting from front left.</td></tr></tbody></table>

## Example Configuration

<pre class="language-json"><code class="lang-json">{
  "imu": {
    <a data-footnote-ref href="#user-content-fn-1">"type": "pigeon2"</a>,
    "id": 13,
    "canbus": "canivore"
  },
  "invertedIMU": true,
  "modules": [
    "frontleft.json",
    "frontright.json",
    "backleft.json",
    "backright.json"
  ]
}
</code></pre>

[^1]: See more information [gyroscope.md](../../devices/gyroscope.md "mention")
