# 🌀 TCL Air Conditioner Integration for Home Assistant (via ESPhome)

TCL air conditioner model with wifi feature usually comes with wifi module that looks like a USB dongle. However, TCL only use standard USB-A connector but change the pinouts to control the AC unit via UART and thus some modifications are needed to connect ESP32 module to the AC unit.


> Original project repo at https://github.com/I-am-nightingale/tclac  
> Project blog in russian https://dzen.ru/a/ZmdoyUNswXWnulhg

---

## 🛠️ What you need

- **ESP32** (e.g. ESP32-C3, WROOM32, NodeMCU) common ESP32 devkit is recommended for easy flashing via USB.
  - I use this module [ESP32-C3 Super Mini](https://www.aliexpress.com/item/1005005967641936.html?spm=a2g0o.order_list.order_list_main.98.794e1802hxxIy0)
- **USB-A connector or cable**
  - Any standard USB cable with USB-A male on one end should work, like the one that came with your gadgets.
  - USB-A male connector like this [AliExpress-Link](https://www.aliexpress.com/item/1005005776162012.html)
- **Home Assistant with ESPHome (Version 2023.3.0 and up)**

---

## 🔌 Cable connection

| USB-A Pin | Cable color| → ESP32 Pin  |
|-----------|------------|--------------|
| GND       | Black      | VIN/VCC      |
| D+        | Green      | GND          |
| D-        | Gray/White | RXD          |
| VBUS      | Red        | TXD          |

### 🔍 Sample Image
The image show how to connect USB-A male pinout to ESP module with GPIO4 = RX and GPIO3 = TX as in the sample yaml configuration.  

![20250414_174322_preview](https://github.com/user-attachments/assets/7ab13c0d-52cd-4f5b-b859-d43baf84bd7e)






---

## 🧠 Installation in Home Assistant

> Required **ESPHome** add-on in Home Assistant.

### 1. ESPHome installation

- In Home Assistant under **Settings → Add-ons → Add-on store → ESPHome Device Builder**

### 2. Create new device

- In ESPHome-Dashboard → "New Device"
- Select your ESP32 Type e.g. `esp32-c3-devkitm-1` or `nodemcu-32s`

### 3. YAML configuration

#### Option A: Simple configuration
[📄 Sample_conf.yaml](https://github.com/thedesp/tclac/blob/master/Sample_conf.yaml)

#### Option B: Advanceced configuration
[📄 TCL-Conditioner.yaml](https://github.com/thedesp/tclac/blob/master/TCL-Conditioner.yaml)

📝 **Important:**  
- Adjust Wi-Fi data, device name, etc.  
- See comments in YAML for help with setup

### 4. Flash to ESP32

- Connect a USB cable for fist time flash.
- OTA (Over-the-Air) can be used for subsequent flashes.

---

## ✅ Compatible air conditioner model

These models have been successfully tested:

- **TCL:** TAC-07CHSA / TAC-09CHSA / TAC-12CHSA / TAC-12CHDA / **TAC-PRO19**
- **Daichi:** AIR20AVQ1, AIR25AVQS1R-1, DA35EVQ1-1
- **Axioma:** ASX09H1 / ASB09H1
- **Dantex:** RK-12SATI / RK-12SATIE  
- ...and similar models

⚠️ **Note:**  
Even if the model name matches, there may be differences (no USB port, no UART on the board, etc.).

---

## 🔧 Advanced configuration via remote package

You can load the configuration modularly:

```yaml
packages:
  remote_package:
    url: https://github.com/thedesp/tclac.git
    ref: master
    files:
      - packages/core.yaml   # Mandatory
      # - packages/leds.yaml # Optional
    refresh: 30s
