# Proxnet
Wireless IoT Security Testbed based on Raspberry Pi and ESP32


# ProxNet - Wireless IoT Security Testbed

## Overview

ProxNet is a portable testbed designed for authorized wireless IoT security analysis within a controlled lab environment. It utilizes a Raspberry Pi 4 as the central controller, an ESP32 for RFID/NFC and Bluetooth/BLE scanning, and direct SPI connections for nRF24L01+ (2.4GHz) and CC1101 (Sub-GHz) analysis.

**⚠️ Safety Note:** All active radio transmissions (sniffing, replay, jamming) are intended **strictly** for use within a Faraday-shielded lab environment on authorized devices, following established Rules of Engagement (ROE).

## Hardware Components

* **Controller:** Raspberry Pi 4 Model B (4GB)
* **Microcontroller:** ESP32 Dev Module (WROOM, externally powered, UART data link to Pi)
* **RFID/NFC:**
    * MFRC522 (13.56 MHz, via ESP32 SPI)
    * RDM6300 (125 kHz, via ESP32 UART & LLC)
* **2.4 GHz:** nRF24L01+ PA/LNA (via Pi SPI)
* **Sub-GHz:** CC1101 (via Pi SPI - *Note: Module currently suspected faulty*)
* **Wi-Fi:** Monitor-mode capable USB Adapter (e.g., RTL8188EUS based - *Note: Capture script requires compatible adapter & driver*)
* **Power:** Official Raspberry Pi 4 PSU, External 5V for ESP32
* **Interface:** 5" HDMI Touch Display (intended for Kiosk Mode)
* **Logic Leveling:** 8-Channel Bi-Directional Logic Level Converter (LLC)
* **Power Regulation:** LM2596 Buck Converter (for 3.3V Pi radios)
* **Safety:** Physical Kill Switch

## Functionality

* **RFID/NFC Scanning:** Reads and logs UIDs from 13.56MHz and 125kHz tags via ESP32.
* **Bluetooth/BLE Scanning:** Discovers nearby BT Classic and BLE devices via ESP32.
* **nRF24L01+ Sniffing:** Captures raw packets on a specified 2.4GHz channel via Pi.
* **CC1101 Sniffing:** Captures raw packets on Sub-GHz frequencies (e.g., 433MHz) via Pi (*requires working module*).
* **Wi-Fi Capture:** Captures 802.11 packets using `tcpdump` (*requires compatible adapter/driver*).
* **Web UI:** Flask-based interface for starting/stopping scanners/sniffers and viewing recent logs.
* **Data Logging:** Stores results in SQLite database (`logs/proxnet_log.db`) and CSV files (`logs/proxnet_log.csv`). Sniffer outputs go to `.log` files in `logs/`.

## Setup & Usage

*(Detailed setup instructions to be added)*

1.  **Hardware:** Assemble components according to wiring diagrams.
2.  **Pi Setup:** Flash Raspberry Pi OS Lite, install dependencies (`git`, `python3-venv`, `aircrack-ng`, `tshark`, `tcpdump`, `sqlite3`, `lsof`, graphical components if using kiosk mode). Configure SPI/I2C/Serial via `raspi-config`. Set `dumpcap` capabilities. Add user to `dialout` and `wireshark` groups.
3.  **ESP32 Firmware:** Flash the appropriate `.ino` sketch using Arduino IDE with ESP32 board support.
4.  **Clone Repository:** `git clone https://github.com/Nishant-404/Proxnet.git` (replace URL if needed). `cd Proxnet`
5.  **Create Python Environment:** `python3 -m venv venv`
6.  **Activate Environment:** `source venv/bin/activate`
7.  **Install Python Libs:** `pip install -r requirements.txt` (*Note: `requirements.txt` needs to be created*)
8.  **Run Web UI:** `python3 scripts/web_ui.py`
9.  Access UI via browser: `http://<Pi_IP_Address>:5000`
