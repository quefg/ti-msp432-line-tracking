# Selected firmware snapshots

The C and header files in this directory were copied byte-for-byte from the saved coursework project. They are included to make the sensing and actuation path reviewable; this is a curated source selection rather than a complete CCS project export.

## File guide

| Path | Review focus |
|---|---|
| `Lab3_TimerCompare_Motor/Lab3_Timersmain.c` | Active two-centre-sensor line-tracking loop and TimerA1 callback |
| `Lab3_Bump_Reflectance_Systick/Lab3_Sensorsmain.c` | SysTick measurement phases and bump sampling |
| `inc/Reflectance.c`, `.h` | Register-level sensor acquisition and centre-bit extraction |
| `inc/Motor.c`, `.h` | Direction, enable and PWM command interfaces |
| `inc/PWM.c`, `.h` | TimerA0 PWM setup and duty updates |
| `inc/Bump.c`, `.h` | Switch input setup, bit packing and Port 4 ISR |
| `inc/TimerA1.c`, `.h` | Periodic callback and interrupt setup |
| `inc/SysTickInts.c`, `.h` | SysTick reload and priority configuration |

[`source-manifest.json`](source-manifest.json) records SHA-256 hashes of these unchanged files. The original relative directory names are preserved so existing includes remain recognizable.

## Build scope

The original environment was Code Composer Studio targeting MSP432P401R. A complete rebuild also needs its MSP432 device headers/support package, startup and linker files, CCS project configuration and framework dependencies such as `Clock`, `CortexM`, `LaunchPad`, `TExaS` and the applicable `SysTick` module. Those general-purpose dependencies and generated build outputs are not bundled here.

The two application directories contain separate `main()` functions and must not be combined into one build. Historical includes use Windows backslashes and, in places, letter casing that may need adaptation on a case-sensitive host. See the [control-path review](../docs/control-path.md) before attempting hardware reuse.

No build or flash was performed for this source selection. Existing comments, register writes and earlier commented experiments have not been rewritten to imply a more complete implementation.

## Attribution

The files use Jonathan Valvano's MSP432 teaching framework and include original Simplified BSD notices. Course exercise implementations appear alongside framework code; retaining a file does not imply that the entire file or framework is solely authored by this project's student.
