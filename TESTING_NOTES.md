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
- Gyro Stabilization: Balanced
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
3. Confirm the L Gyro / R Gyro selector is visible on the merged Joy-Con icon, then select R Gyro.
4. Confirm gyro moves the mouse.
5. Confirm A performs mouse left click.
6. Confirm B performs mouse right click.
7. Confirm the left stick moves the cursor while gyro mode is active.
8. Confirm the right stick scrolls a browser page, document, or file list.
9. Confirm Mouse Move Stick and Scroll Wheel Stick cannot stay assigned to the same stick.
10. Confirm optical Joy-con Mouse mode can stay OFF.
11. Toggle gyro OFF with Capture or Chat and confirm A/B mappings plus explicit Mouse Move Stick and Scroll Wheel Stick still work.
12. Press and release A/B repeatedly and confirm the short gyro click-suppression window prevents cursor jumps without making stick movement feel delayed.
13. Switch between L Gyro and R Gyro and confirm the old gyro side does not keep moving the cursor after the side changes.
14. Minimize or hide the app to the system tray for several minutes and confirm gyro, A/B mappings, Mouse Move Stick, and Scroll Wheel Stick still work.
15. Check idle/background CPU while connected. With no stick or gyro movement, CPU should be much lower than the previous fixed 1000 Hz polling behavior.
16. Set Gyro Stabilization to Balanced, hold the active Joy-Con still, and confirm small hand tremor is reduced without making fast cursor movement feel delayed.
17. Switch Gyro Stabilization to Stable and confirm it is steadier but slightly less responsive than Balanced.
18. Switch Gyro Stabilization to Off and confirm the previous gyro mouse behavior is restored.

## Notes

- Joy-Con optical mouse mode remains controlled by the existing Joy-con Mouse setting and is not required for the gyro stick mouse or stick wheel features.
- 6-Axis air mouse uses the low-power direct gyro path. 9-Axis still uses sensor fusion and is expected to cost more CPU.
- Gyro Stabilization only affects Windows gyro mouse movement. Off preserves the previous behavior; Balanced is the recommended air-mouse starting point; Stable favors click stability and reading/scrolling tasks.
- The Gyro trigger controls gyro sensor cursor movement only. Explicit stick mouse, stick wheel, and mouse-button mappings remain active when gyro sensor movement is toggled off.
- Switching L Gyro / R Gyro resets the gyro sensor activation state so the previous side cannot keep a stale gyro mouse state.
- Mouse control over elevated windows such as an administrator Task Manager may still be blocked by Windows integrity-level isolation unless the app itself is elevated. This project should not elevate itself or install extra mouse drivers.
- Stick Assist is still separate from the explicit Mouse Move Stick and Scroll Wheel Stick settings. When either explicit stick feature is enabled, the legacy Stick Assist cursor contribution is skipped to avoid hidden stick movement conflicting with scroll or explicit cursor movement.
- Horizontal wheel events are sent when Windows exposes `MOUSEEVENTF_HWHEEL`; otherwise only vertical wheel movement is expected.
