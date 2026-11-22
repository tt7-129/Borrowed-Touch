# Saved-control-logic reference (derived from the supplied `.hd` files)

> **Important:** This is a readable transcription of Blockly/Tianwen saved blocks, not compiled or executable ASRPRO firmware. Where behaviour depends on the editor or board runtime, it is marked as unverified.

## Speech-only diagnostic project

```
Configure voice Olivia, volume=10, speed=12
Configure wake phrase "Hey Buddy" (configured ID 0)
Configure command "shake hands" (configured ID 1)
Configure command "put down" (configured ID 2)
Configure Serial TX=PB_5, RX=PB_6, baud=9600
On ASR_CODE():
    switch recognized_id:
        0: Serial.println("HeyBuddy")
        1: Serial.println("shakehands")
        2: Serial.println("putdown")
```

## Integrated project — preserved block logic

```
startup:
    css = 0                          // int8_t control mode
    configure same wake phrase and voice commands

application_thread:
    servo_write(pin=13, angle=170)
    servo_write(pin=14, angle=10)
    chaoshengbo = 0                  // int16_t distance value
    ci = 0                           // int8_t one-shot flag

    repeat_forever:
        delay(100)

        if css == 1:
            for j from 0 through 160 by 1:
                servo_write(13, 10 + (170 - j))
                servo_write(14, 10 + j)
                delay(10)
            for j from 0 through 160 by 1:
                servo_write(13, 170 - (170 - j))
                servo_write(14, 170 - j)
                delay(10)

        else if css == 0:
            chaoshengbo = ultrasonic_getdistance(TRIG=0, ECHO=1)

            if (chaoshengbo < 100) AND (ci == 0):
                ci = 1
                for i from 0 through 1 by 1:
                    for j from 0 through 160 by 1:
                        servo_write(13, 10 + (170 - j))
                        servo_write(14, 10 + j)
                        delay(10)
                    for j from 0 through 160 by 1:
                        servo_write(13, 170 - (170 - j))
                        servo_write(14, 170 - j)
                        delay(10)

            if (chaoshengbo > 100) AND (ci == 1):
                ci = 0

ASR_CODE callback (saved switch cases):
    case 100: css = 0
    case 1:   css = 1
    case 2:   css = 2

sleep hook:
    css = 0
```

The `for` wording "through" follows common inclusive Blockly semantics but should be confirmed in the Tianwen code generation. The distance sensor's units, the task scheduler, and the specific `ASR_CODE` event mapping are not encoded in this transcription.

## Points that must not be silently changed

- The saved proximity threshold is **100**, not 180.
- The saved integrated servo pin fields are **13 and 14**, not literal `PB_5` and `PB_6`.
- The saved first servo formula is **`10 + (170 - j)`** and the second sweep's first servo formula is **`170 - (170 - j)`**. Do not rewrite either formula as a smoother or symmetric movement without identifying it as a new implementation.
- The saved distance trigger uses logical **AND**. The flag is reset only when the measured value is **strictly greater than** 100 (not `>=`).
- The saved proximity branch contains an outer loop `i: 0 to 1` with step 1; it might execute the gesture **twice** under inclusive loop semantics. Therefore a thesis statement claiming exactly one physical cycle needs a runtime/code-generator check.
- The `put down` branch changes `css` to 2; it does **not** contain a direct servo-return command.
- The voice branch repeats its movement every main-loop iteration as long as `css == 1`.
