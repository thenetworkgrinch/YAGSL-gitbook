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
5. THEN [flip the modules](#user-content-fn-1)[^1].
6. THEN uninvert all of the drive motors by setting them to `false`
7. THEN set `invertIMU` to `true`.
8. THEN set your drive motors inversion states to `true`.

{% hint style="danger" %}
IF none of these work you most likely have a incorrect hardware configuration, something is not working as expected, or something is wired incorrectly.&#x20;
{% endhint %}

[^1]: Change the device configuration for the drive AND angle motor controllers AND the absolute encoders in the following way.\
    \
    Front Left   -> Back Right\
    Front Right -> Back Left\
    Back Left    -> Front Right\
    Back Right  -> Front Left
