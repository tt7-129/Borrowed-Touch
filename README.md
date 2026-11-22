# Borrowed Touch

**Investigating Social Contact through a Responsive, Non-Human Robotic Handshake**  
MSc Creative Robotics · Creative Computing Institute · University of the Arts London

## Overview

Borrowed Touch is a practice-led research project using a visibly non-human, segmented robotic limb to explore how a familiar handshake can be interpreted as social contact. The study compares two initiation conditions: a requested handshake using offline voice recognition and an approach-triggered handshake using ultrasonic distance sensing.

This repository contains the **original Tianwen graphical-programming project files** (`.hd`) supplied by the project author. These are XML-based block-workspace files, **not Arduino `.ino` sketches** or automatically generated, compilable C/C++ code. To inspect or edit the original blocks, open them in the compatible Tianwen environment. Their behaviour has been documented by inspecting the saved blocks; actual running behaviour must be checked on the hardware.

## Original source files

| File | Purpose | Confirmed content |
| --- | --- | --- |
| `firmware/speech_diagnostic/BorrowedTouch_Speech_Diagnostic.hd` | Early speech-recognition / serial diagnostic project | Wake phrase and two commands; `Serial` configured on PB_5/PB_6 at 9600 baud; command-ID labels sent via serial |
| `firmware/integrated_control/BorrowedTouch_Integrated_Control.hd` | Integrated interaction project | Voice-command state, two servo blocks, ultrasonic distance measurement, paired-servo routines, one-shot proximity trigger |

### Voice commands in both projects

- `Hey Buddy` — configured as wake phrase, recognition ID `0`.
- `shake hands` — configured command, recognition ID `1`.
- `put down` — configured command, recognition ID `2`.

The integrated project's `ASR_CODE` switch cases are **100 → `css = 0`**, **1 → `css = 1`**, and **2 → `css = 2`**. It is important not to confuse the configured wake-phrase ID `0` with the separate switch case `100` in the saved project. The exact interpretation of case `100` should be verified in Tianwen/ASRPRO documentation.

## Integrated control configuration (as saved in the `.hd` blocks)

| Item | Saved value |
| --- | --- |
| Control-state variable | `css` (`int8_t`, initialized to `0`) |
| Proximity one-shot flag | `ci` (`int8_t`, initialized to `0`) |
| Distance variable | `chaoshengbo` (`int16_t`, initialized to `0`) |
| Servo output pin fields | `13`, `14` |
| Initial servo angles | pin `13`: `170°`; pin `14`: `10°` |
| Ultrasonic TRIG/ECHO pin fields | `0`, `1` |
| Distance threshold | `100` (unit not encoded in this `.hd` file) |
| Main application loop delay | `100` (delay-unit interpretation requires environment documentation) |
| Movement loop | counter `j` from `0` to `160` in steps of `1`; per-step delay field `10` |
| Distance trigger | `(chaoshengbo < 100) AND (ci == 0)` |
| Re-arm trigger | `(chaoshengbo > 100) AND (ci == 1)` |

The `css = 1` branch repeatedly executes the paired-servo movement while that state remains active; the `css = 0` branch checks proximity. The `css = 2` command changes the state; **there is no dedicated return-to-rest servo command in the corresponding switch branch**. See `documentation/CONTROL_LOGIC_REFERENCE.md` for a source-faithful presentation of the saved block sequence.

**Pin naming note:** The earlier diagnostic file explicitly selects `PB_5`/`PB_6` for **serial TX/RX**, whereas the integrated file selects numeric pin fields **13/14** for the two servos. The integrated ultrasonic block selects **0/1**. Do not claim these are physically the same pins as PB5/PB6 and PA0/PA1 until the pin mapping is checked against the actual board / Tianwen IDE.

## Study design and findings

Six participants experienced both modes and completed five-point ratings and structured interviews. The thesis reports higher mean comfort and control ratings in the voice-requested condition, and higher mean social connection and perceived robot initiative in the proximity-triggered condition. These are **exploratory/descriptive results**, not statistical evidence of a general population effect.

## Repository navigation

- [`firmware/speech_diagnostic`](firmware/speech_diagnostic): original voice-and-serial diagnostic Tianwen project.
- [`firmware/integrated_control`](firmware/integrated_control): original integrated Tianwen project.
- [`documentation/CONTROL_LOGIC_REFERENCE.md`](documentation/CONTROL_LOGIC_REFERENCE.md): readable explanation of the saved blocks.
- [`documentation/TECHNICAL_VERIFICATION_CHECKLIST.md`](documentation/TECHNICAL_VERIFICATION_CHECKLIST.md): points to verify with the working device and in the thesis before submission.

## Reproducing or editing the project

1. Use the Tianwen graphical programming environment compatible with these `.hd` projects and the actual ASRPRO board model.
2. Open the `.hd` files; review the command vocabulary, saved pin fields, and motor limits before connecting servos.
3. Verify the physical wiring and actual pin-number-to-port mappings on the board in hand.
4. If running on hardware, use a suitable separate servo power supply, secure the arm, and test movement with no participant hand in its path before use.

The native `.hd` file contents have been preserved byte-for-byte; only the filenames and surrounding documentation have been changed to English. Some internal block labels and variable names remain as in the original Tianwen workspaces to avoid altering the saved program. No fabricated build logs, runtime measurements, GitHub URL, CAD sources or video files are included.

## Thesis reference

If this repository is published, cite its **actual repository URL** in the thesis Appendix C, along with `README.md`. Replace any placeholder URL only after publication. If additional CAD files, diagrams, demo videos or source exports become available, add only verified original files and update the README accordingly.
