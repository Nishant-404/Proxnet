# ProxNet Wiring Diagram

This document details the connections between the Raspberry Pi 4, ESP32, radio modules, and peripherals for the ProxNet project.

**⚠️ Important Notes:**
* **Common Ground:** ALL `GND` pins must be connected to a single common ground point (e.g., a breadboard rail connected to Pi `Pin 9`).
* **ESP32 Power:** The ESP32 is powered **externally** via its Micro USB port using a separate 5V adapter. Its USB port is **NOT** connected to the Pi.
* **Kill Switch:** The physical kill switch interrupts the 5V power going *from* the Pi (Pin 2) *to* the ESP32 (5V pin) and the LM2596 (IN+).
* **Logic Level Converter (LLC):** Used to safely connect 5V signals (RDM6300 TX) to the 3.3V ESP32 GPIO.
* **LM2596:** Calibrated to output 3.3V to power the nRF24L01+ and CC1101.
* **CC1101:** Currently suspected faulty and disabled in software, but wiring is included for completeness.
* **Wi-Fi Adapter:** Connects to a Pi USB 2.0 port (not shown in diagram below).

## Connection Diagram (Mermaid)

```mermaid
graph TD
    subgraph Power
        A[External USB 5V] --> ESP32(ESP32);
        B[Pi Official 5V] --> KillSwitch(Kill Switch);

        KillSwitch -- 5V --> RPi4(Raspberry Pi 4);
        KillSwitch -- 5V --> Pi_5V_Rail(Pi 5V Rail for Peripherals);

        Pi_5V_Rail --> RDM_VCC(RDM6300 VCC);
        Pi_5V_Rail --> LLC_HV(LLC HV);
        Pi_5V_Rail --> LM2596_IN(LM2596 Buck IN);

        LM2596_IN -- Calibrated 3.3V --> RadioPwr3V3(High-Current 3.3V Rail);
        RadioPwr3V3 --> NRF_VCC(nRF24 VCC);
        RadioPwr3V3 --> CC_VCC(CC1101 VCC);

        ESP32 -- 3.3V Pin --> LLC_LV(LLC LV);
        ESP32 -- 3.3V Pin --> RC522_VCC(RC522 VCC);
    end

    subgraph Ground (Common Rail)
        RPi4 -- Pin 9 (GND) --> GND_Rail(Common GND);
        ESP32 -- GND Pin --> GND_Rail;
        LLC_GND(LLC GND x2) --> GND_Rail;
        RC522_GND(RC522 GND) --> GND_Rail;
        RDM_GND(RDM6300 GND) --> GND_Rail;
        LM2596_GND(LM2596 GND) --> GND_Rail;
        NRF_GND(nRF24 GND) --> GND_Rail;
        CC_GND(CC1101 GND) --> GND_Rail;
    end

    subgraph Pi Connections
        RPi4 -- SPI0 (Pins 19,21,23) --> NRF24(nRF24 SPI);
        RPi4 -- SPI0 (Shared) --> CC1101(CC1101 SPI - Faulty);
        RPi4 -- SPI CS (Pin 26 CE1) --> NRF_CSN(nRF24 CSN);
        RPi4 -- SPI CS (Pin 24 CE0) --> CC_CSN(CC1101 CSN);
        RPi4 -- GPIO (Pin 15) --> NRF_CE(nRF24 CE);
        RPi4 -- USB Port --> WiFi_Adapter(Wi-Fi USB Adapter);
    end

    subgraph ESP32 Connections
        ESP32 -- GPIO 17 (TX2, 3.3V) --> RPi4(Pin 10 RXD, 3.3V);
        RDM_TX(RDM6300 TX, 5V) --> LLC_HV1(LLC HV1);
        LLC_LV1(LLC LV1, 3.3V) --> ESP32(GPIO 16 RX2);
        RC522(RC522 SPI) -- GPIO 27,5,18,19,23 (3.3V) --> ESP32;
    end


















    
