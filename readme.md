# Cloud Tic ⏰
**ESP8266 NTP Clock v1.0**   ![Visitors](https://visitor-badge.laobi.icu/badge?page_id=skvhacks.cloud_tic)

A Wi-Fi connected desk or bedroom clock built on the ESP8266 (Wemos D1 Mini / NodeMCU) with a TM1637 4-digit display, active buzzer alarms, live weather temperature, and a clean Minimal web UI — all configurable over your local network with no app required.



## Features

- 🕐 **NTP time sync** — always accurate, no RTC chip needed
- 🌡️ **Live weather** — fetches temperature from OpenWeatherMap and cycles it on the display
- ⏰ **3 independent alarms** — per-day scheduling, 4 tone patterns, auto-stop (after 60 sec. you can customize it in portal)
- 🌙 **Night mode** — auto-dims the display between set hours
- 📡 **Web UI** — full settings panel at `http://192.168.x.xxx`.
- 🔄 **OTA firmware update** — flash new firmware from the browser, no USB cable needed
- 💾 **EEPROM persistence** — all settings survive power cuts
- 📶 **WiFiManager** — first-boot captive portal for Wi-Fi setup, no hardcoded credentials
- 💡 **Status LED** — solid during AP mode, fast blink when Wi-Fi is lost
- 📟 **IP scroll on boot** — scrolls `IP 192 168 x xxx` across the display after connecting



## Hardware

| Component | Details |
|-----------|---------|
| Microcontroller | ESP8266 — Wemos D1 Mini or NodeMCU |
| Display | TM1637 4-digit 7-segment |
| Buzzer | Active buzzer (active LOW) |
| Status LED | Built-in LED on D4 (active LOW) |
| Power | USB 5V |

## Wiring

| Component | ESP8266 Pin | GPIO |
|-----------|-------------|------|
| TM1637 CLK | D5 | GPIO14 |
| TM1637 DIO | D6 | GPIO12 |
| TM1637 VCC | 3.3V or 5V | — |
| TM1637 GND | GND | — |
| Buzzer (+) | D7 | GPIO13 |
| Buzzer (−) | GND | — |


> **Note:** The buzzer are **active LOW** — they activate when the pin is driven LOW.
> If your buzzer draws more than ~12 mA, add an NPN transistor (BC547 / 2N2222) between D7 and the buzzer with a 1 kΩ base resistor.



## Flashing the Firmware

1. Download `cloud_tic.bin` from the [Releases](../../releases) page
2. Install [bin Flash Tool](https://github.com/nodemcu/nodemcu-flasher) 
3. Select the file and flash it

## First Boot Setup

1. Power on the device — display shows ` AP `
2. The status LED turns **solid ON**
3. On your phone or laptop, connect to Wi-Fi: **`Clock_Setup`** / password: **`12345678`**
4. A captive portal will open automatically. If it doesn't, open a browser and go to`http://192.168.4.1`. — select your home Wi-Fi and enter the password
5. Device restarts, connects, and scrolls the IP address across the display
6. Status LED turns **OFF** — device is ready



## Web Interface

Open a browser and go to:
- **`http://cloudtic.local`** — if your router supports mDNS
- **`http://192.168.x.x`** — the IP shown on the display at boot

### Settings sections

| Section | Controls |
|---------|---------|
| **Time** | Live clock, sync status, weather badge, Sync Now button |
| **Display** | Brightness (0–7), 12H/24H format, blink colon, night mode with start/end hour |
| **Time Zone** | UTC offset slider (−12 to +14, step 0.5) |
| **Alarms** | 3 alarms — time picker, enable toggle, day selector (Sun–Sat) |
| **Sound** | Tone pattern, auto-stop duration, Stop button |
| **Weather** | Enable toggle, API key, City ID, Celsius/Fahrenheit, Refresh button |
| **Firmware** | OTA update (upload .bin), Restart button |



## Weather Setup

1. Sign up at [openweathermap.org](https://openweathermap.org) (free tier is enough)
2. Copy your API key
3. Find your city ID at [openweathermap.org/find](https://openweathermap.org/find) — search your city and note the number in the URL (e.g. `1263659` for Mannargudi)
4. In the web UI → Weather section:
   - Paste your API key
   - Enter the city ID
   - Choose Celsius or Fahrenheit
   - Enable the toggle
   - Press **Refresh** to test

When enabled, the display cycles: **time for 10 seconds → temperature for 3 seconds → repeat**.

Temperature format on display:
```
24°C  →  [2][4][°][C]
 7°F  →  [ ][7][°][F]
-5°C  →  [-][5][°][C]
```

## Alarm Tones

| ID | Name | Pattern |
|----|------|---------|
| 0 | Classic Beep | 500 ms on / 500 ms off |
| 1 | Fast Beep | 100 ms on / 100 ms off |
| 2 | Double Beep | beep-beep, 700 ms pause |
| 3 | Siren | 50 ms rapid on/off |

During any alarm tone, the **display blinks at a fixed 500 ms rate** regardless of which tone is selected. The alarm auto-stops after the configured duration (default 60 seconds). You can also stop it instantly from the web UI or by Reseting the device.



## OTA Firmware Update

No USB cable needed after the first flash:

1. In the web UI → Firmware → click **Update**
2. Select your compiled `.bin` file
3. Click **Upload & Flash**
4. Device flashes and reboots automatically

## Status LED Behaviour

| State | LED |
|-------|-----|
| Connecting / AP mode | Solid ON |
| Normal operation | OFF |
| Wi-Fi lost | Fast blink (150 ms) |


## License

MIT — free to use, modify, and distribute.


## Credits

- [tzapu/WiFiManager](https://github.com/tzapu/WiFiManager)
- [arduino-libraries/NTPClient](https://github.com/arduino-libraries/NTPClient)
- [bblanchon/ArduinoJson](https://github.com/bblanchon/ArduinoJson)
- [avishorp/TM1637](https://github.com/avishorp/TM1637)
- [OpenWeatherMap API](https://openweathermap.org/api)

