# The eight steps

## Something is wrong but it's driving-ish?

When the inversion state changes do not work and nothing else you know of fixes your issue's the debugging of such setups can be painful and arduous. Here are the eight steps that are unfortunately necessary to accomplish debugging this.&#x20;

Affectionally known as _"Translational Axis change based off of the robot heading"_ (the X and Y axis change in field oriented based off your robot turning.) these steps will resolve it somewhere along the way.

## The eight steps.

{% hint style="danger" %}
**The steps are dangerous!**

6 of them will cause your robot to spin out of control.

1 of them will be correct.

1 of them will appear to be correct but have the translational axis change based off of the robot heading.
{% endhint %}

{% hint style="warning" %}
You are not expected to complete all of these steps to achieve a functioning swerve drive, somewhere along the way your issue should disappear.&#x20;
{% endhint %}

1. Start by setting `invertIMU` in `swervedrive.json` to `false` **AND** the `drive` `invert` to `false` in module JSONs.
2. THEN set `invertIMU` to `true`.
3. THEN invert all of the drive motors in the module JSONs by setting them to `true`.
4. THEN set `invertIMU` to `false`.
5.  THEN [flip the modules](#user-content-fn-1)[^1].\


    <figure><img src="../.gitbook/assets/image-48.png" alt=""><figcaption><p>Depiction of flipping the modules correctly.</p></figcaption></figure>
6. THEN uninvert all of the drive motors by setting them to `false`
7. THEN set `invertIMU` to `true`.
8. THEN set your drive motors inversion states to `true`.

{% hint style="danger" %}
IF none of these work you most likely have a incorrect hardware configuration, something is not working as expected, or something is wired incorrectly.&#x20;
{% endhint %}

<details>

<summary>How to swap module configurations</summary>

For the examples we label with numbers as to be less confused, however when changing module files around we assign the numbers to the respective initial module configuration names. For the example above we have as follows

1. `frontleft.json`
2. `frontright.json`
3. `backleft.json`
4. `backright.json`

#### Swapping `frontleft.json` with `backright.json`

<pre class="language-json" data-title="frontleft.json"><code class="lang-json">{
<strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 4,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 3,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 9,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": -114.609,
</strong>  "location": {
    "front": 12,
    "left": 12
  }
}
</code></pre>

<pre class="language-json" data-title="backright.json"><code class="lang-json"><strong>{
</strong><strong>  "drive": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 5,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "angle": {
</strong><strong>    "type": "sparkmax",
</strong><strong>    "id": 6,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "encoder": {
</strong><strong>    "type": "cancoder",
</strong><strong>    "id": 11,
</strong><strong>    "canbus": null
</strong><strong>  },
</strong><strong>  "inverted": {
</strong><strong>    "drive": false,
</strong><strong>    "angle": false
</strong><strong>  },
</strong><strong>  "absoluteEncoderOffset": -18.281,
</strong>  "location": {
    "front": -12,
    "left": -12
  }
}
</code></pre>

Swap the highlighted lines and you have swapped the module configurations correctly.

## The easy way

1. Change the location negations to the desired module side.
2. Rename the files without overwriting eachother.

</details>

[^1]: Change the device configuration for the drive AND angle motor controllers AND the absolute encoders in the following way.\
    \
    Front Left   -> Back Right\
    Front Right -> Back Left\
    Back Left    -> Front Right\
    Back Right  -> Front Left
