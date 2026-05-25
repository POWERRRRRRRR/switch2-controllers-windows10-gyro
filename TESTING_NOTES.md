# Testing Notes

## Source Startup

From PowerShell in the repository root:

```powershell
conda activate joycon2dev
$env:PYTHONPATH = "$PWD\src"
python .\src\gui.py
```

For development-only smoke tests where driver checks must be skipped:

```powershell
conda activate joycon2dev
$env:PYTHONPATH = "$PWD\src"
$env:SWITCH2_SKIP_DRIVER_CHECK = "1"
python .\src\gui.py
```

## Manual Settings

- Driver: WinUHid
- Joy-con Mouse: OFF
- Built-in Gyro: 6-Axis
- Activation: Toggle
- Capture or Chat: Gyro
- A: Mouse Left Click
- B: Mouse Right Click
- Mouse Move Stick: Left Stick
- Mouse Move Sensitivity: 5.0
- Mouse Move Deadzone: 0.15
- Scroll Wheel Stick: Right Stick
- Scroll Sensitivity: 1.0
- Scroll Deadzone: 0.20
- Stick Assist: 0.0

## Manual Validation

1. Start the app.
2. Connect Joy-Con 2 through the app, not Windows Bluetooth settings.
3. Confirm R Gyro is selected.
4. Confirm gyro moves the mouse.
5. Confirm A performs mouse left click.
6. Confirm B performs mouse right click.
7. Confirm the left stick moves the cursor while gyro mode is active.
8. Confirm the right stick scrolls a browser page, document, or file list.
9. Confirm Mouse Move Stick and Scroll Wheel Stick cannot stay assigned to the same stick.
10. Confirm optical Joy-con Mouse mode can stay OFF.

## Notes

- Joy-Con optical mouse mode remains controlled by the existing Joy-con Mouse setting and is not required for the gyro stick mouse or stick wheel features.
- Stick Assist is still separate from the explicit Mouse Move Stick and Scroll Wheel Stick settings. When either explicit stick feature is enabled, the legacy Stick Assist cursor contribution is skipped to avoid hidden stick movement conflicting with scroll or explicit cursor movement.
- Horizontal wheel events are sent when Windows exposes `MOUSEEVENTF_HWHEEL`; otherwise only vertical wheel movement is expected.
