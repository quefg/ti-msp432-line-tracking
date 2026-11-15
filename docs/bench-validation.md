# Bench validation and evidence

## Evidence included

- An original photograph of the assembled TI robot on the laboratory bench.
- Selected C source showing reflectance GPIO acquisition, centre-bit extraction, timer PWM and the motor-command branches.
- A separate interrupt-driven sensing example with configured SysTick timing.
- Original source headers and a SHA-256 manifest for the copied files.

The photograph establishes the physical build context. It does not establish sensor timing, motor speed, tracking accuracy or collision-stop reliability. The source establishes the recorded implementation, not a newly observed hardware result.

## Suggested checks before the next demonstration

These are proposed checks; they are not a record of passed tests.

| Check | Procedure | Evidence to save |
|---|---|---|
| Initialization and pin mapping | Confirm the active board mapping and explicitly initialize reflectance hardware in the motor application. | Wiring photo, build configuration and initialization trace |
| Reflectance polarity | Place each centre sensor over the line and background; record `00`, `01`, `10` and `11`. | Raw bit table under stated lighting and sensor height |
| Motor mapping | With wheels raised, issue each command at a modest duty value and verify wheel direction. | Command-to-wheel table and short video |
| Line-loss response | Lift or move the sensor away from the line and observe the stop path. | Video plus timestamped sensor/command log |
| Bump behaviour | After resolving competing motor-enable writes, press each switch while issuing motion commands. | Per-switch response and stop-latency measurements |
| Timing | Observe sampling and PWM on appropriate diagnostic pins with a logic analyser. | Captures with clock, timer and firmware configuration |
| Repeatability | Repeat the same route with controlled battery state, surface, lighting and speed setting. | Run count, completion count and failure notes |

## Result reporting

Record firmware revision, robot setup, battery voltage, surface, ambient lighting, sensor height, command values and number of repetitions. Report failures alongside successful runs. Describe an elapsed delay or configured timer period as such until it has been measured on hardware.

No new robot tests, automated hardware tests or performance measurements were run for this portfolio package.
