# HA-ESPHome-WVC-Inverter
 
> **ESPHome-Firmware für KaiDeng WVC-Life Mikro-Wechselrichter** — direkte Home Assistant Integration ohne Cloud, ohne App.
 
[![ESPHome](https://img.shields.io/badge/ESPHome-compatible-blue?logo=esphome)](https://esphome.io/)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-integration-41BDF5?logo=home-assistant)](https://www.home-assistant.io/)
[![Hardware](https://img.shields.io/badge/Hardware-ESP--07S%20%2F%20ESP8266-red)](https://www.espressif.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Release](https://img.shields.io/badge/Release-1.0%20(Feb%202025)-orange)](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/releases/tag/1.0)
 
---
 
## Überblick
 
Dieses Projekt ersetzt das originale **HF-LPT270 WLAN-Modul** in den KaiDeng WVC-Life Mikro-Wechselrichtern durch einen **ESP-07S (ESP8266)**, der mit einer eigenen ESPHome-Firmware geflasht wird.
 
Nach dem Umbau sendet der Wechselrichter seine Messwerte **direkt und lokal an Home Assistant** — vollständig ohne Cloudverbindung, ohne die proprietäre App „Cloud Intelligence" und ohne Abhängigkeit von externen Servern.
 
**Vorher:** Wechselrichter → AliCloud → Cloud Intelligence App → (ggf. HA)
**Nachher:** Wechselrichter → ESP-07S (ESPHome) → Home Assistant (lokal)
 
![WVC800 Home Assistant Dashboard](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/raw/main/Pictures/WVC800_HA-Page.png)
 
---
 
## Inhaltsverzeichnis
 
- [Unterstützte Geräte](#unterstützte-geräte)
- [Nicht unterstützte Geräte](#nicht-unterstützte-geräte)
- [Repository-Struktur](#repository-struktur)
- [Voraussetzungen](#voraussetzungen)
- [Hardware-Umbau](#hardware-umbau)
- [Wichtiger Hinweis: Überhitzung](#wichtiger-hinweis-überhitzung)
- [ESPHome YAML-Konfiguration](#esphome-yaml-konfiguration)
- [Anpassung der Standardwerte](#anpassung-der-standardwerte)
- [Features der Firmware](#features-der-firmware)
- [Home Assistant Integration](#home-assistant-integration)
- [Ordnerübersicht](#ordnerübersicht)
- [Fehlerbehebung](#fehlerbehebung)
- [Haftungsausschluss & Lizenz](#haftungsausschluss--lizenz)
 
---
 
## Unterstützte Geräte
 
Das Projekt unterstützt die WVC-Inverter der **„Wireless Series R3"**, die mit dem WLAN-Modul **HF-LPT270** ausgestattet sind:
 
| Modell            | Max. Leistung |
|-------------------|--------------|
| WVC-600W-Life     | 600 W        |
| WVC-700W-Life     | 700 W        |
| WVC-800W-Life     | 800 W        |
| WVC-1200W-Life    | 1200 W       |
| WVC-1400W-Life    | 1400 W       |
| WVC-1600W-Life    | 1600 W       |
| WVC-2000W-Life    | 2000 W       |
| WVC-2400W-Life    | 2400 W       |
| WVC-2800W-Life    | 2800 W       |
 
> **Erkennungsmerkmal:** Das HF-LPT270 Modul ist auf der Platine deutlich sichtbar beschriftet. Modelle der „Wireless Series R3" verwenden dieses Modul standardmäßig.
 
---
 
## Nicht unterstützte Geräte
 
Ältere WVC-Wechselrichter-Modelle, die ein **433 MHz Kommunikationsmodul** anstelle des HF-LPT270 WLAN-Moduls verwenden, werden von diesem Projekt **nicht unterstützt** und sind mit diesem Ansatz nicht kompatibel.
 
---
 
## Repository-Struktur
 
```
HA-ESPHome-WVC-Inverter/
├── ESPHome/                  # YAML-Konfigurationsdateien für ESPHome
│   ├── wvc600_esphome.yaml   # Konfiguration für WVC-600W
│   ├── wvc700_esphome.yaml   # Konfiguration für WVC-700W
│   └── wvc800_esphome.yaml   # Konfiguration für WVC-800W
├── HF-LPT270-WiFi-BT/        # Informationen zum Original-WLAN-Modul
├── PCB-Design/               # PCB-Design-Dateien für den Umbau
├── Pictures/                 # Screenshots und Fotos
│   └── WVC800_HA-Page.png    # Beispiel HA-Dashboard
├── WVC-Datasheets/           # Datenblätter der Wechselrichter
├── LICENSE                   # MIT-Lizenz
└── README.md                 # Diese Datei
```
 
---
 
## Voraussetzungen
 
### Software
 
- **Home Assistant** (beliebige aktuelle Version)
- **ESPHome Add-on** in Home Assistant (oder ESPHome CLI)
- Python / ESPHome-Compiler-Umgebung zum Erstellen der Firmware
 
### Hardware
 
| Komponente         | Details                                              |
|--------------------|------------------------------------------------------|
| **ESP-07S**        | ESP8266-Modul mit externer Antenne (Ersatz für HF-LPT270) |
| **Thermalpads**    | Dringend empfohlen zur Kühlung (s. Hinweis unten)   |
| **Lötequipment**   | Für die Montage auf der Inverter-Platine             |
| **FTDI-Adapter**   | Zum erstmaligen Flashen des ESP-07S (USB-to-Serial)  |
 
---
 
## Hardware-Umbau
 
Der Umbau besteht darin, das originale **HF-LPT270 WLAN-Modul** auf der Inverter-Hauptplatine auszulöten und durch einen mit ESPHome geflashten **ESP-07S** zu ersetzen.
 
### Schritte (Kurzübersicht)
 
1. **Wechselrichter vom Netz trennen** und sicherstellen, dass keine Spannung anliegt (Solaranlage abklemmen oder abdecken).
2. **Gehäuse öffnen** und Platine zugänglich machen.
3. **HF-LPT270 Modul auslöten** – dieses ist in der Regel ein SMD-Modul auf der Platine.
4. **ESP-07S vorher flashen** (vor dem Einbau, da danach kein einfacher Zugang mehr besteht) – ESPHome-Firmware aufspielen via FTDI-Adapter.
5. **ESP-07S einlöten** auf den freigewordenen Modulplatz (Pinkompatibilität beachten, ggf. Adapterplatine aus dem `PCB-Design/`-Ordner verwenden).
6. **Thermalpads anbringen** (dringend empfohlen, s. u.).
7. **Gehäuse schließen** und Wechselrichter in Betrieb nehmen.
 
> Detaillierte Fotos und PCB-Layouts befinden sich in den Ordnern `Pictures/` und `PCB-Design/`.
 
---
 
## Wichtiger Hinweis: Überhitzung
 
Mehrere Nutzer haben über **instabile Netzwerkkommunikation und andere Instabilitäten** bei WVC-Mikrowechselrichtern berichtet. Viele dieser Probleme sind auf eine **unkontrollierte Überhitzung des HF-LPT270 WLAN-Moduls** zurückzuführen.
 
Das WLAN-Modul erzeugt zusätzliche Wärme und ist **nicht mit einem Kühlelement** im Inneren des Wechselrichtergehäuses verbunden.
 
**Empfehlung:** Es wird dringend geraten, so viele **Thermalpads** auf dem Modul anzubringen, bis eine thermische Verbindung zum Aluminiumdeckel des Gehäuses hergestellt ist. Dies sorgt für ausreichende Kühlung und verhindert Überhitzung im Normalbetrieb.
 
> ⚠️ Dieselbe Empfehlung gilt natürlich auch für den Ersatz-ESP-07S!
 
---
 
## ESPHome YAML-Konfiguration
 
Im Ordner `ESPHome/` befinden sich drei Beispiel-YAML-Konfigurationen für den **ESP-07S**:
 
- `wvc600_esphome.yaml` — für WVC-600W
- `wvc700_esphome.yaml` — für WVC-700W
- `wvc800_esphome.yaml` — für WVC-800W
 
Diese YAML-Dateien dienen als Ausgangsbasis und müssen an die eigenen Einstellungen angepasst werden (siehe nächsten Abschnitt).
 
### Beispielstruktur einer YAML-Konfiguration
 
```yaml
esphome:
  name: wvc800
  friendly_name: WVC-800 Micro Inverter
 
esp8266:
  board: esp07s
 
# WLAN-Zugangsdaten
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
 
  # Fallback Hotspot (Captive Portal) wenn WLAN nicht verfügbar
  ap:
    ssid: "WVC800 Fallback Hotspot"
    password: "fallback_password"
 
captive_portal:
 
# OTA-Updates und Webserver
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
 
# UART-Kommunikation mit dem Wechselrichter-Mikrocontroller
uart:
  id: uart0
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600  # ggf. je nach Modell anpassen
 
# Sensoren (Beispiel)
sensor:
  - platform: template
    name: "PV Power"
    unit_of_measurement: "W"
    # ...
 
# Home Assistant Dienst zum Senden von Befehlen an den Inverter
button:
  - platform: template
    name: "WVC800 Write UART0"
    # ...
```
 
---
 
## Anpassung der Standardwerte
 
Nachdem die passende YAML-Datei heruntergeladen wurde, müssen **mehrere Standardwerte** an die eigene Installation angepasst werden:
 
| Parameter                  | Beschreibung                                              |
|----------------------------|-----------------------------------------------------------|
| `wifi.ssid`                | SSID des eigenen WLAN-Netzwerks                          |
| `wifi.password`            | WLAN-Passwort                                            |
| `api.encryption.key`       | API-Verschlüsselungsschlüssel für Home Assistant         |
| `ota.password`             | Passwort für Over-the-Air Updates                        |
| Wechselrichter-Leistung    | Nennleistung des Modells (600/700/800 W etc.)            |
| UART Baudrate              | Ggf. je nach Firmware-Version des Wechselrichters        |
| Inverter-spezifische Werte | Gerätespezifische Kalibrierungswerte aus dem Datenblatt  |
 
> Die jeweiligen Stellen im YAML-Code sind mit Kommentaren markiert, um die Anpassung zu erleichtern.
 
---
 
## Features der Firmware
 
Die ESPHome-Firmware bietet folgende Funktionen:
 
### Webserver-Interface
- **Lokales Web-Interface** für den Online-Status des Moduls
- **OTA (Over-the-Air) Updates** direkt über den Webbrowser – kein erneutes Öffnen des Gehäuses notwendig
 
### WLAN Fallback Hotspot
- Wenn die Verbindung zum konfigurierten WLAN nicht hergestellt werden kann, öffnet das Modul automatisch einen **eigenen WLAN-Hotspot** (Access Point)
- Über diesen Hotspot lassen sich WLAN-Zugangsdaten komfortabel neu eingeben
 
### Home Assistant Dienst
- Jede Firmware stellt einen **Home Assistant Service** bereit:
  - WVC-600: `wvc600_write_uart0`
  - WVC-700: `wvc700_write_uart0`
  - WVC-800: `wvc800_write_uart0`
- Über diesen Dienst können **Befehle direkt an den Mikrocontroller des Wechselrichters** gesendet werden (z. B. Leistungslimitierung, Statusabfragen)
 
### Übertragene Messwerte (Beispiele)
 
| Sensor                        | Einheit |
|-------------------------------|---------|
| PV-Eingangsleistung           | W       |
| PV-Eingangsspannung           | V       |
| PV-Eingangsstrom              | A       |
| AC-Ausgangsleistung           | W       |
| AC-Ausgangsspannung           | V       |
| AC-Netzfrequenz               | Hz      |
| Gesamte eingespeiste Energie  | kWh     |
| Tagesertrag                   | kWh     |
| Gerätetemperatur              | °C      |
| Betriebsstatus / Fehlercodes  | —       |
 
---
 
## Home Assistant Integration
 
Nach erfolgreichem Flashen und Einbau verbindet sich der ESP-07S automatisch mit dem konfigurierten WLAN. Home Assistant erkennt das neue ESPHome-Gerät in der Regel **automatisch per mDNS**.
 
### Manuelle Einrichtung (falls automatisch nicht erkannt)
 
1. In Home Assistant: **Einstellungen → Geräte & Dienste → Integration hinzufügen**
2. „ESPHome" auswählen
3. IP-Adresse oder Hostname des ESP-07S eingeben (z. B. `wvc800.local`)
4. API-Verschlüsselungsschlüssel eingeben (aus der YAML-Datei)
 
### Dashboard-Beispiel
 
Nach der Integration stehen alle Sensoren direkt in Home Assistant zur Verfügung und können in beliebigen Dashboards (z. B. mit Lovelace-Karten oder dem Energy-Dashboard) visualisiert werden.
 
![Home Assistant WVC800 Dashboard](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/raw/main/Pictures/WVC800_HA-Page.png)
 
---
 
## Ordnerübersicht
 
### `ESPHome/`
Enthält die drei YAML-Beispielkonfigurationen für ESP-07S:
- `wvc600_esphome.yaml`
- `wvc700_esphome.yaml`
- `wvc800_esphome.yaml`
 
### `HF-LPT270-WiFi-BT/`
Dokumentation und Informationen zum originalen HF-LPT270 WLAN-Modul, das im Wechselrichter verbaut ist. Hilfreich zum Verständnis der Hardware-Schnittstelle und der UART-Kommunikation.
 
### `PCB-Design/`
PCB-Layouts und Designdateien für eine mögliche Adapterplatine, die den Einbau des ESP-07S in den Modulplatz des HF-LPT270 erleichtert.
 
### `Pictures/`
Fotos und Screenshots – unter anderem das Beispiel-Dashboard für den WVC-800 in Home Assistant sowie ggf. Bilder vom Hardware-Umbau.
 
### `WVC-Datasheets/`
Datenblätter der unterstützten WVC-Wechselrichter-Modelle als Referenz für technische Spezifikationen, Pinbelegungen und Kommunikationsprotokolle.
 
---
 
## Fehlerbehebung
 
### Wechselrichter sendet keine Daten an HA
- UART-Baudrate in der YAML-Datei prüfen (je nach WVC-Modell und Firmware-Version kann sie variieren)
- Verkabelung des ESP-07S auf der Platine überprüfen (TX/RX korrekt)
- ESPHome-Logs über das Web-Interface oder HA einsehen
 
### Instabile WLAN-Verbindung / Abstürze
- Thermalpads anbringen (Überhitzung ist die häufigste Ursache!)
- WLAN-Signalstärke prüfen – der Wechselrichter benötigt ein stabiles 2,4-GHz-Netzwerk
- ESP-07S mit externer Antenne verwenden und Antenne aus dem Metallgehäuse herausführen, falls möglich
 
### Fallback Hotspot erscheint
- Das Modul konnte keine Verbindung zum WLAN herstellen
- Mit dem Hotspot-WLAN verbinden und die Zugangsdaten über das Captive-Portal neu eingeben
 
### OTA-Update schlägt fehl
- Sicherstellen, dass Gerät und PC im selben Netzwerk sind
- OTA-Passwort in der YAML-Datei prüfen
- Ggf. per FTDI-Adapter und seriellem Anschluss neu flashen
 
### Bekannte Einschränkungen
- **Ältere WVC-Modelle mit 433-MHz-Modul werden nicht unterstützt** – nur Modelle der „Wireless Series R3" mit HF-LPT270
- Das Schreiben von Befehlen an den Inverter (z. B. Leistungslimitierung) ist über den bereitgestellten HA-Dienst möglich, erfordert aber ggf. weitere Analyse des Kommunikationsprotokolls
 
---
 
## Haftungsausschluss & Lizenz
 
Dieses Projekt ist ein **Hobby-Projekt** und steht in **keiner Verbindung zu einem Unternehmen**.
 
**Nutzung auf eigene Gefahr:** Der Umbau des Wechselrichters erfordert Eingriffe in Elektronik, die an das Stromnetz angeschlossen ist. Unsachgemäße Arbeiten können zu Schäden am Gerät, Garantieverlust oder Personenschäden führen. Vor dem Öffnen des Gehäuses **unbedingt alle Spannungsquellen trennen** (Netz und Solarpanele).
 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 
Lizenz: **MIT License** — siehe [LICENSE](LICENSE)
 
---
 
## Links & Ressourcen
 
- 🔗 [Repository auf GitHub](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter)
- 📦 [Release 1.0 (Februar 2025)](https://github.com/GernotAlthammer/HA-ESPHome-WVC-Inverter/releases/tag/1.0)
- 🏠 [Home Assistant](https://www.home-assistant.io/)
- ⚡ [ESPHome Dokumentation](https://esphome.io/)
- 📡 [HF-LPT270 Datenblatt](http://www.hi-flying.com/hf-lpt270)
- 💬 [Home Assistant Community Forum](https://community.home-assistant.io/)
- 📷 [Ähnliches Projekt: WVC-Life Data Collector](https://github.com/damianburrin/WVC-Life-Edition-Data-Collector)
 
---
 
*Zuletzt aktualisiert: 02.02.2025 | Autor: GernotAlthammer*
 
