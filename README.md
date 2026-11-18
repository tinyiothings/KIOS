# KIOS — A tiny operating system for the KIO cybercharm

KIOS is a self-contained firmware for the **ESP32-S3** that turns a 128×128
round-ish LCD pendant into a pocket companion: a virtual pet (KIO), a GIF
player, a scrolling-text display, a set of mini-games, and — yes — Doom.

It is designed to be **hackable**, **battery-friendly**, and **fully
configurable from a web browser** over Wi-Fi. No app, no cable, no toolchain
required to change the GIFs or the ticker: just open the page.

---

## Features

- **KIO**, a virtual cat that lives on a black screen and reacts to sound
  (ES8311 microphone). Five moods, blinking, rolling, pouncing, sleeping.
- **GIF player** with up to 60 GIFs stored in LittleFS, adjustable speed,
  and a custom boot animation.
- **Ticker** with three animations (scroll, typewriter, fade), multiple
  messages, six fonts, and a configurable color.
- **Mini-apps**: social battery, counter, stopwatch, flashlight.
- **Arcade**: six games (taxi, flappy, doodle, tetris, racer, snow).
- **Doom** (optional, requires a WAD file). Upload your own `doom1.wad`
  from the web UI.
- **Web settings**: accent color, eye color, brightness, GIF speed,
  ticker playlist, storage usage, OTA-free firmware reboot.
- **ESP-NOW gamepad** support for all games and Doom.
- **Battery monitoring** (LiPo 1S) and USB/charging detection.

---

## Hardware

The reference board is the **Movecall Moji / Spotpear ESP32-S3-0.85-AI**
(or any ESP32-S3 with 8 MB PSRAM and 16 MB flash).

| Peripheral      | Pin / bus                        |
|-----------------|----------------------------------|
| LCD GC9107      | SPI: DC 42, CS 41, SCLK 40, MOSI 39, RST 38 |
| Backlight       | GPIO 14 (PWM)                    |
| Battery ADC     | GPIO 1 (ADC1_CH0)                |
| USB detect      | GPIO 9 (digital, 20k/30k divider)|
| Microphone      | ES8311 via I2S (GPIO 8/4/6/5/3)  |
| BOOT button     | GPIO 0                           |

Flash layout (see `partitions.csv`): 4 MB app (factory), 12 MB LittleFS
for GIFs and the Doom WAD.

---

## Building

### Arduino IDE

1. Install **ESP32 Arduino Core 3.x** (tested with 3.3.2).
2. Select board **ESP32S3 Dev Module**.
3. Set:
   - Flash size: **16 MB**
   - Partition scheme: **Custom** (use the provided `partitions.csv`)
   - PSRAM: **OPI PSRAM**
   - CPU frequency: **240 MHz**
4. Install libraries: `Arduino_GFX`, `lvgl` (8.4.x), `ESP-DOOM` (optional).
5. Open `src/main.ino` and upload.

### PlatformIO (coming soon)

A `platformio.ini` will be provided in a future release.

---

## Web interface

1. On first boot, KIOS connects to the Wi-Fi network defined in
   `config.h` (`WIFI_SSID` / `WIFI_PASS`).
2. The IP address is shown on the screen.
3. Open `http://<ip>/` from any phone or computer on the same network.

From there you can change colors, brightness, ticker text, upload GIFs,
upload a Doom WAD, and reboot the device.

---

## License

KIOS is released under the **GNU General Public License v2.0 or later**.
See `LICENSE` and `NOTICE` for details.

Doom is a trademark of id Software. The WAD files are not distributed
with KIOS.

---

## Contributing

Pull requests are welcome. Please keep the code commented in English and
respect the existing style (no dynamic allocation in hot paths, prefer
`inline` functions in headers, keep the binary small).
