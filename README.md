FSR Interfacing with Arduino UNO R4
===================================

Short description
-----------------
This small project reads an analog Force Sensitive Resistor (FSR) value on an Arduino UNO R4 and prints the raw ADC value to the serial console.

Files of interest
-----------------
- `platformio.ini` — PlatformIO project configuration (board, framework, build options).
- `src/main.cpp` — Example firmware: reads analog pin `A0`, prints `FSR Value: <0-1023>` every 500 ms.

Quick contract
-------------
- Inputs: A Force Sensitive Resistor wired as a voltage divider into analog pin A0.
- Outputs: Serial messages printed at 9600 baud with the raw ADC reading.
- Error modes: Serial port not connected / wrong COM port; bad wiring causing always-zero or saturated readings.

Hardware wiring (recommended)
-----------------------------
Use the FSR as the top resistor in a voltage divider.

Connections (textual):
- 5V --> FSR --> A0
- A0 --> 10k resistor --> GND
- GND --> Arduino GND

Notes:
- You can swap the FSR and fixed resistor positions; just ensure the analog pin reads the divider node.
- Typical fixed resistor value: 10 kΩ (experiment between 4.7k and 100k depending on sensor range).

What `platformio.ini` contains (summary)
----------------------------------------
`platformio.ini` configures PlatformIO for the target board and framework. Typical keys:
- `env:` — environment (board selection). For Arduino UNO R4 it should reference the appropriate board id.
- `platform` and `framework` — platform core and `arduino` framework.
- Build flags, upload options, and monitor settings can be added or tuned here.

If you need to change the board or upload port, edit `platformio.ini`.

How to build, upload and monitor (PlatformIO CLI / PowerShell)
---------------------------------------------------------------
Run these commands in the project root (PowerShell):

```powershell
# build
pio run

# build and upload (auto-detect port or set in platformio.ini)
pio run -t upload

# open serial monitor at 9600 baud
pio device monitor -b 9600
```

If you prefer the PlatformIO extension in VS Code or CLion PlatformIO plugin, use the GUI "Build", "Upload", and "Serial Monitor" actions.

About `src/main.cpp`
--------------------
High-level behavior:
- Initializes serial at 9600 baud in `setup()`.
- Reads the analog value `A0` with `analogRead()` inside `loop()`.
- Prints `FSR Value: <value>` then waits 500 ms.

To change behavior:
- Adjust `fsrPin` to another analog pin.
- Change `Serial.begin(9600)` to a different baud rate — remember to update the monitor command.
- Reduce `delay(500)` to sample faster.

Sample serial output
--------------------
FSR Value: 0
FSR Value: 12
FSR Value: 67
FSR Value: 350
... (values in the 0–1023 ADC range)

Troubleshooting
---------------
- Serial monitor shows no output:
  - Ensure the board is connected and the correct COM/serial port is selected.
  - Check that `Serial.begin(9600);` baud matches the monitor.

- Values are always 0 or always 1023:
  - Check wiring and that the fixed resistor is in place for a proper divider.
  - Confirm `fsrPin` matches the pin you're reading.

- Build/upload errors:
  - Confirm `platformio` is installed and on PATH (run `pio --version`).
  - Confirm `platformio.ini` specifies the correct `board` for your UNO R4.

Extending the project
---------------------
- Map raw ADC values to a useful range (e.g., 0–100) with `map()` or normalize to voltage.
- Add calibration and thresholds to detect presses and gestures.
- Log values to an SD card or stream over serial to a PC app.

License
-------
MIT — copy, modify, and use freely.

If you'd like, I can also:
- add a dedicated `README_src_main.md` or split documentation into multiple files,
- update `platformio.ini` to include a `monitor_speed` or explicit `upload_port` entry,
- or add a small Arduino-style comment header to `src/main.cpp` for quick reference.

