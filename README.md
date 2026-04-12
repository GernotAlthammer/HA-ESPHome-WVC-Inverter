# HA-ESPHome-WVC-Inverter
 
> **ESPHome firmware for KaiDeng WVC-Life Micro Inverters** — direct Home Assistant integration, no cloud, no app required.
 
[![ESPHome](https://img.shields.io/badge/ESPHome-compatible-blue?logo=esphome)](https://esphome.io/)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-integration-41BDF5?logo=home-assistant)](https://www.home-assistant.io/)
[![Hardware](https://img.shields.io/badge/Hardware-ESP--07S%20%2F%20ESP8266-red)](https://www.espressif.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Release](https://img.shields.io/badge/Release-1.0%20(Feb%202025)-orange)](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/releases/tag/1.0)
 
---
 
## Overview
 
This project replaces the original **HF-LPT270 Wi-Fi module** inside KaiDeng WVC-Life micro inverters with an **ESP-07S (ESP8266)** running custom ESPHome firmware.
 
After the hardware swap, the inverter sends its live measurements **directly and locally to Home Assistant** — with no cloud connection, no proprietary "Cloud Intelligence" app, and no dependency on external servers.
 
**Before:** Inverter → AliCloud → Cloud Intelligence App → (optionally HA)
**After:** Inverter → ESP-07S (ESPHome) → Home Assistant (local only)
 
![WVC800 Home Assistant Dashboard](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/raw/main/Pictures/WVC800_HA-Page.png)
 
---
 
## Table of Contents
 
- [Supported Devices](#supported-devices)
- [Unsupported Devices](#unsupported-devices)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Hardware Modification](#hardware-modification)
- [Important: Overheating Warning](#important-overheating-warning)
- [ESPHome YAML Configuration](#esphome-yaml-configuration)
- [Customising Default Values](#customising-default-values)
- [Firmware Features](#firmware-features)
- [Home Assistant Integration](#home-assistant-integration)
- [Folder Reference](#folder-reference)
- [Troubleshooting](#troubleshooting)
- [Disclaimer & License](#disclaimer--license)
 
---
 
## Supported Devices
 
This project supports WVC inverters of the **"Wireless Series R3"** generation that are equipped with the **HF-LPT270 Wi-Fi module**:
 
| Model             | Max. Power |
|-------------------|-----------|
| WVC-600W-Life     | 600 W     |
| WVC-700W-Life     | 700 W     |
| WVC-800W-Life     | 800 W     |
| WVC-1200W-Life    | 1200 W    |
| WVC-1400W-Life    | 1400 W    |
| WVC-1600W-Life    | 1600 W    |
| WVC-2000W-Life    | 2000 W    |
| WVC-2400W-Life    | 2400 W    |
| WVC-2800W-Life    | 2800 W    |
 
> **How to identify your module:** The HF-LPT270 module is clearly labelled on the inverter PCB. All "Wireless Series R3" models use this module by default.
 
---
 
## Unsupported Devices
 
Older WVC inverter models that use a **433 MHz communication module** instead of the HF-LPT270 Wi-Fi module are **not supported** and are incompatible with this approach.
 
---
 
## Repository Structure
 
```
HA-ESPHome-WVC-Inverter/
├── ESPHome/                  # ESPHome YAML configuration files
│   ├── wvc600_esphome.yaml   # Configuration for WVC-600W
│   ├── wvc700_esphome.yaml   # Configuration for WVC-700W
│   └── wvc800_esphome.yaml   # Configuration for WVC-800W
├── HF-LPT270-WiFi-BT/        # Documentation for the original Wi-Fi module
├── PCB-Design/               # PCB design files for the adapter board
├── Pictures/                 # Screenshots and photos
│   └── WVC800_HA-Page.png    # Example Home Assistant dashboard
├── WVC-Datasheets/           # Inverter datasheets for reference
├── LICENSE                   # MIT License
└── README.md                 # This file
```
 
---
 
## Prerequisites
 
### Software
 
- **Home Assistant** (any recent version)
- **ESPHome Add-on** installed in Home Assistant, or the ESPHome CLI
- ESPHome compiler environment to build and flash the firmware
 
### Hardware
 
| Component               | Details                                                          |
|-------------------------|------------------------------------------------------------------|
| **ESP-07S**             | ESP8266 module with external antenna (replacement for HF-LPT270) |
| **Thermal pads**        | Strongly recommended to prevent overheating (see warning below)  |
| **Soldering equipment** | Required for mounting the module on the inverter PCB             |
| **FTDI adapter**        | USB-to-serial adapter for the initial firmware flash             |
 
---
 
## Hardware Modification
 
The modification consists of desoldering the original **HF-LPT270 Wi-Fi module** from the inverter mainboard and replacing it with an **ESP-07S** running ESPHome firmware.
 
### Steps (Summary)
 
1. **Disconnect the inverter from the grid** and ensure no voltage is present — disconnect and shade the solar panels as well.
2. **Open the housing** to gain access to the main PCB.
3. **Desolder the HF-LPT270 module** — it is typically an SMD module soldered directly to the board.
4. **Flash the ESP-07S before installation** (access after installation is difficult) — upload the ESPHome firmware using an FTDI adapter via USB-to-serial.
5. **Solder the ESP-07S** onto the vacated module footprint. Check pin compatibility and use the adapter PCB from the `PCB-Design/` folder if needed.
6. **Apply thermal pads** (strongly recommended — see below).
7. **Close the housing** and power the inverter back on.
 
> Detailed photos and PCB layouts are available in the `Pictures/` and `PCB-Design/` folders.
 
---
 
## Important: Overheating Warning
 
Multiple users have reported **unstable network communication and other instability issues** with WVC micro inverters. Many of these problems are caused by **uncontrolled overheating of the HF-LPT270 Wi-Fi module**.
 
The Wi-Fi module generates additional heat and is **not connected to any cooling element** inside the inverter housing.
 
**Recommendation:** It is strongly advised to apply as many **thermal pads** to the module as needed to establish a thermal connection to the aluminium housing lid. This ensures sufficient cooling to prevent overheating during normal operation.
 
> ⚠️ The same recommendation applies to the replacement ESP-07S module!
 
---
 
## ESPHome YAML Configuration
 
The `ESPHome/` folder contains three example YAML configurations for the **ESP-07S**:
 
- `wvc600_esphome.yaml` — for WVC-600W
- `wvc700_esphome.yaml` — for WVC-700W
- `wvc800_esphome.yaml` — for WVC-800W
 
These YAML files serve as a starting point and must be customised to match your own setup (see the next section).
 
### Example Configuration Structure
 
```yaml
esphome:
  name: wvc800
  friendly_name: WVC-800 Micro Inverter
 
esp8266:
  board: esp07s
 
# Wi-Fi credentials
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
 
  # Fallback hotspot (captive portal) if Wi-Fi is unavailable
  ap:
    ssid: "WVC800 Fallback Hotspot"
    password: "fallback_password"
 
captive_portal:
 
# OTA updates and web server
ota:
  password: !secret ota_password
 
web_server:
  port: 80
 
# Home Assistant API
api:
  encryption:
    key: !secret api_encryption_key
 
# Logging
logger:
 
# UART communication with the inverter microcontroller
uart:
  id: uart0
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600  # adjust if needed depending on your model
 
# Sensors (example)
sensor:
  - platform: template
    name: "PV Power"
    unit_of_measurement: "W"
    # ...
 
# Home Assistant service to send commands to the inverter
button:
  - platform: template
    name: "WVC800 Write UART0"
    # ...
```
 
---
 
## Customising Default Values
 
After downloading the appropriate YAML file, **several default values must be adjusted** to match your installation:
 
| Parameter                   | Description                                                   |
|-----------------------------|---------------------------------------------------------------|
| `wifi.ssid`                 | Your home Wi-Fi network name                                  |
| `wifi.password`             | Your Wi-Fi password                                           |
| `api.encryption.key`        | API encryption key for Home Assistant                         |
| `ota.password`              | Password for over-the-air firmware updates                    |
| Inverter rated power        | Nominal output power of your model (600/700/800 W, etc.)      |
| UART baud rate              | May vary depending on inverter firmware version               |
| Inverter-specific values    | Device-specific calibration values from the datasheet         |
 
> All adjustable fields are clearly marked with comments in the YAML files to make customisation straightforward.
 
---
 
## Firmware Features
 
### Web Server Interface
- **Local web interface** for viewing the device's online status
- **OTA (Over-the-Air) updates** directly via the browser — no need to reopen the housing after initial installation
 
### Wi-Fi Fallback Hotspot
- If the module cannot connect to the configured Wi-Fi network, it automatically opens its own **Wi-Fi access point**
- Connect to this hotspot to re-enter Wi-Fi credentials via a captive portal
 
### Home Assistant Service
Each firmware exposes a dedicated **Home Assistant service** to send commands directly to the inverter's microcontroller:
 
| Model    | HA Service Name      |
|----------|----------------------|
| WVC-600W | `wvc600_write_uart0` |
| WVC-700W | `wvc700_write_uart0` |
| WVC-800W | `wvc800_write_uart0` |
 
This can be used for actions such as power output limiting or status queries.
 
### Transmitted Sensor Values (Examples)
 
| Sensor                        | Unit |
|-------------------------------|------|
| PV input power                | W    |
| PV input voltage              | V    |
| PV input current              | A    |
| AC output power               | W    |
| AC output voltage             | V    |
| AC grid frequency             | Hz   |
| Total energy fed to grid      | kWh  |
| Daily energy yield            | kWh  |
| Device temperature            | °C   |
| Operating status / fault codes | —   |
 
---
 
## Home Assistant Integration
 
Once the ESP-07S is flashed and installed, it will automatically connect to the configured Wi-Fi network. Home Assistant will usually **detect the new ESPHome device automatically via mDNS**.
 
### Manual Setup (if not auto-discovered)
 
1. In Home Assistant: **Settings → Devices & Services → Add Integration**
2. Search for and select **ESPHome**
3. Enter the IP address or hostname of the ESP-07S (e.g. `wvc800.local`)
4. Enter the API encryption key from your YAML configuration
 
### Dashboard Example
 
Once integrated, all sensors are immediately available in Home Assistant and can be used in any dashboard — including the built-in **Energy Dashboard** for solar production tracking.
 
![Home Assistant WVC800 Dashboard](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/raw/main/Pictures/WVC800_HA-Page.png)
 
---
 
## Folder Reference
 
### `ESPHome/`
Contains the three YAML example configurations for the ESP-07S:
- `wvc600_esphome.yaml`
- `wvc700_esphome.yaml`
- `wvc800_esphome.yaml`
 
### `HF-LPT270-WiFi-BT/`
Documentation and information about the original HF-LPT270 Wi-Fi module built into the inverter. Useful for understanding the hardware interface and UART communication protocol.
 
### `PCB-Design/`
PCB layouts and design files for an optional adapter board that simplifies installing the ESP-07S into the HF-LPT270 module footprint.
 
### `Pictures/`
Photos and screenshots — including the example Home Assistant dashboard for the WVC-800 and photos from the hardware modification process.
 
### `WVC-Datasheets/`
Datasheets for the supported WVC inverter models, providing technical specifications, pin assignments, and communication protocol details.
 
---
 
## Troubleshooting
 
### Inverter not sending data to Home Assistant
- Verify the UART baud rate in the YAML file (can vary by model and inverter firmware version)
- Double-check the ESP-07S wiring on the PCB (TX/RX must be connected correctly)
- Review ESPHome logs via the web interface or Home Assistant
 
### Unstable Wi-Fi connection or random crashes
- Apply thermal pads — **overheating is the most common root cause!**
- Check Wi-Fi signal strength — the inverter requires a stable 2.4 GHz network
- Use the ESP-07S with an external antenna and route it out of the metal housing if possible
 
### Fallback hotspot appears on startup
- The module could not connect to the configured Wi-Fi network
- Connect to the fallback hotspot and re-enter Wi-Fi credentials via the captive portal page
 
### OTA update fails
- Ensure the device and your computer are on the same local network
- Check the OTA password in your YAML configuration
- If all else fails, re-flash via the FTDI adapter and serial connection
 
### Known Limitations
- **Older WVC models with 433 MHz modules are not supported** — only "Wireless Series R3" models with the HF-LPT270
- Sending commands to the inverter (e.g. power limiting) is possible via the exposed HA service, but may require further analysis of the communication protocol for advanced use cases
 
---
 
## Disclaimer & License
 
This is a **hobby project** and is in **no way affiliated with any company**.
 
**Use at your own risk.** Modifying the inverter requires working with electronics connected to the mains power supply. Improper work can result in device damage, voided warranty, or personal injury. Always **disconnect all voltage sources** (mains grid and solar panels) before opening the housing.
 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 
License: **MIT License** — see [LICENSE](LICENSE)
 
---
 
## Links & Resources
 
- 🔗 [Repository on GitHub](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter)
- 📦 [Release 1.0 (February 2025)](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/releases/tag/1.0)
- 🏠 [Home Assistant](https://www.home-assistant.io/)
- ⚡ [ESPHome Documentation](https://esphome.io/)
- 📡 [HF-LPT270 Datasheet](http://www.hi-flying.com/hf-lpt270)
- 💬 [Home Assistant Community Forum](https://community.home-assistant.io/)
- 📷 [Related Project: WVC-Life Data Collector](https://github.com/damianburrin/WVC-Life-Edition-Data-Collector)
 
---
 
*Last updated: 02 February 2025 | Author: GernotAlthammer*
 
