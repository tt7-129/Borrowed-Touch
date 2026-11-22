# R13: English Naming and Original-Source Integrity

All **repository paths and filenames** in this package are in English.

| Native-file role | English filename | Modification |
|---|---|---|
| Speech/serial diagnostic Tianwen workspace | `firmware/speech_diagnostic/BorrowedTouch_Speech_Diagnostic.hd` | Filename only |
| Integrated robot Tianwen workspace | `firmware/integrated_control/BorrowedTouch_Integrated_Control.hd` | Filename only |

The native `.hd` files were copied byte-for-byte. Their internal block text and variable names were **not translated or rewritten**, because such changes could affect the original project. Use the English documentation to explain the saved blocks rather than presenting a new pseudocode file as compiled firmware.

## Upload

1. Open or create a GitHub repository named `Borrowed-Touch`.
2. Upload `README.md`, `firmware/`, and `documentation/` from the **inside** of this folder, not the outer folder or ZIP.
3. Verify the README is shown on the repository home page and the two `.hd` files appear under their English filenames.
4. Add the **real** GitHub URL to Section 4.3 and Appendix C of the thesis. Never leave an example username or a non-existent video link in the submitted report.
5. Do not upload photographs or interviews of identifiable participants to a public repository without appropriate permission.

## Verification before submitting the thesis

The saved integrated file uses servo pin fields `13` and `14`, ultrasonic pin fields `0` and `1`, and a threshold of `100`. The earlier speech diagnostic file uses `PB_5` and `PB_6` for serial communication. The correspondence between saved numeric fields and physical board labels has not been established. The saved proximity routine contains an outer loop and voice state may repeat gestures. Check actual runtime before claiming that each activation performs exactly one cycle.
