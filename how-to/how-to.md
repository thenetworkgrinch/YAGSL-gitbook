# How-to

Goal-oriented recipes for a robot that's already running YAGSL. If you're setting up your first
swerve robot, start with the [Tutorial](../tutorial/README.md) section instead — these guides
assume a working config and address a specific task or problem.

- [How to tune PIDF gains](tune-pidf-gains.md)
- [How to determine inversion](determine-inversion.md)
- [How to verify your module locations](verify-module-locations.md)
- [How to use a custom gyro](use-a-custom-gyro.md) — for hardware YAGSL's parser doesn't build,
  like a roboRIO-attached Studica AHRS
- [How to drive to a pose](drive-to-a-pose.md) — a single point-to-point PID move, no path planner
  needed
- [How to set up PathPlanner](setup-pathplanner.md) — register a YAGSL-built `SwerveDrive` with
  PathPlanner's `AutoBuilder`
- [The 8 steps](the-8-steps.md) — the translational-axis/heading
  coupling debugging procedure
- [How to fix common SparkMAX/SparkFlex problems](fix-sparkmax-common-problems.md)
- [How to set up AdvantageScope](set-up-advantagescope.md)
- [How to view your DataLog in AdvantageScope](view-your-datalog-in-advantagescope.md) — opening a
  recorded `.wpilog` after the fact, not live telemetry
- [Troubleshooting flowchart](troubleshooting-flowchart.md)
