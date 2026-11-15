# Control path and source review

This note describes the active code in the saved coursework files. It separates implemented logic from behaviour that still needs a fresh bench check.

## Line tracking

[`Lab3_Timersmain.c`](../firmware/Lab3_TimerCompare_Motor/Lab3_Timersmain.c) configures a 48 MHz system clock, the LaunchPad interface, bump inputs, motors, TExaS and a periodic TimerA1 callback. The active foreground loop uses two centre reflectance bits:

```text
Eight reflectance inputs on Port 7
    → charge / discharge measurement
    → (data >> 3) & 0x03
    → forward / left / right / stop command
    → direction GPIO + TimerA0 compare values
    → motor driver
```

The application branches are `11 → forward`, `10 → left`, `01 → right`, and `00 → stop`. They are discrete rules rather than a PID controller. The source includes commented earlier experiments; those blocks are not active features.

[`Reflectance_Center()`](../firmware/inc/Reflectance.c) passes the requested wait time to `Reflectance_Read()`. That function enables illumination, charges the sensor lines for 10 µs, switches Port 7 to inputs, waits, reads the port and disables illumination. The application requests a 1000 µs wait. These are configured delays, not externally measured timing results.

## Separate interrupt-driven sensing example

[`Lab3_Sensorsmain.c`](../firmware/Lab3_Bump_Reflectance_Systick/Lab3_Sensorsmain.c) calls `Reflectance_Init()` and configures `SysTick_Init(48000, 1)` after setting the clock to 48 MHz.

| Tick within cycle | ISR work |
|---|---|
| 0 | `Reflectance_Start()` |
| 1 | `Reflectance_End()` and `Bump_Read()` |
| 2–9 | Advance the counter |

The configured interrupt interval is 1 ms; the measurement cycle repeats every 10 ms. This is a separate lab application, not an integrated scheduler in the line-tracking program.

## Active driver mapping

| Interface | Active source mapping |
|---|---|
| Reflectance inputs | P7.0–P7.7 |
| Reflectance illumination | P5.3 and P9.2 with `RSLK_MAX = 1` |
| Motor direction | P5.4 and P5.5 with `RSLK_MAX = 1` |
| Motor enable | P3.6 and P3.7 |
| Motor PWM | P2.6 / TA0 CCR3 and P2.7 / TA0 CCR4 |
| Bump inputs | P4.0, P4.2, P4.3, P4.5, P4.6, P4.7 |

The table follows active register writes in the included drivers. Some inherited application comments refer to older P1 motor-direction and P8 bump mappings, so the comments alone should not be used as a wiring authority. Actual wheel/channel polarity should be checked on the robot.

## Observations for a future bench revision

- The saved motor application's `main()` does not call `Reflectance_Init()`, although the driver documents that precondition. Add and validate explicit initialization before treating this snapshot as a runnable reference.
- The TimerA1 callback clears motor-enable bits when `Bump_Read() != 0x3F`, then sets them when no switch is pressed. Foreground `Motor_Forward()`, `Motor_Left()` and `Motor_Right()` also enable the motors. There is no latched stop state or arbitration between those writes. Reliable collision inhibition needs a deliberate state/control policy and a bench test.
- `Bump_Init()` enables Port 4 interrupts, but the included Port 4 handler reads the switches and clears flags; it does not command a motor stop.
- `TimerA1_Stop()` writes the disable bit for interrupt 12, while `TimerA1_Init()` enables interrupt 10. The stop helper is not called in the shown application, but it should be reviewed before reuse.
- The historical files are preserved without corrective edits so their provenance remains clear. These review observations are not claims that fixes have been implemented or validated.

No physical validation was performed when preparing this portfolio.
