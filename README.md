# ICE-V Wireless
Combined ESP32C3 and iCE40 FPGA board

<img src="docs/ice-v_front.jpg" width="640" />
<img src="docs/ice-v_back.jpg" width="640" />

## Abstract
This project combines an Espressif ESP32-C3-MINI-1 (which includes 4MB of
flash in the module) with a Lattice iCE40UP5k-SG48 FPGA to allow WiFi and
Bluetooth control of the FPGA. ESP32 and FPGA I/O is mostly uncommitted
except for the pins used for SPI communication between ESP32 and FPGA.
Several of the ESP32C3 GPIO pins are available for additonal interfaces
such as serial, ADC, I2C, etc.

This project comprises both hardware, firmware, gateware and host-side
communication utilities.

## Hardware
The [Hardware](Hardware) directory provides the schematic and PCB layout in
KiCAD 6 format.
The design provides:
* Three standard PMOD connectors spaced to accommodate
dual-PMOD modules with all eight signal pins directly connected to the
FPGA.
  * Many PMOD signals are available in differential pairs with the positive
and negative halves on adjacent odd/even pins.
  * PMOD power pins can be tied either to the on-board regulated 3.3V supply
(default) or to the unregulated ~4V supply (requires cutting a trace and adding
a solder blob).
* Additional I/O connector with seven ESP32C3 GPIO lines and one FPGA
line, along with power, ground and reset connections.
* 8MB PSRAM connected directly to the FPGA.
* USB-C connector for power, programming and JTAG debugging of the ESP32C3.
* Tactile buttons for reset and boot mode selection of the ESP32C3.
* RGB LED directly driven by the iCE40 FPGA
* LED indicators for power, charging, FPGA configuration and ESP32C3 software.
* LiPo Battery operation and charging.
* Onboard antenna for WiFi and Bluetooth.

### Pinout
<img src="docs/pinout.png" width="640" />

### Schematic
A PDF of the schematic is here: [ICE-V-Wireless Schematic](docs/esp32c3_fpga_schematic.pdf)

## Firmware

[![.github/workflows/build-firmware.yml](https://github.com/ICE-V-Wireless/ICE-V-Wireless/actions/workflows/build-firmware.yml/badge.svg)](https://github.com/ICE-V-Wireless/ICE-V-Wireless/actions/workflows/build-firmware.yml)

The [Firmware](Firmware) directory provides the full source for the ESP32 firmware
that is loaded into the ICE-V-Wireless board from the factory. The ESP32 firmware
is written in C with the ESP-IDF toolchain and librarie and provides both a USB
and a TCP socket interface over WiFi with the following features:
* Initial loading of the FPGA configuration at powerup from a SPIFFS filesystem
contained in the ESP32C3F[H/N]4 flash. 
<br/>(See [Page 9 of the ESP32-C3 Series Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf#page=9)
for part number details).
* USB and WiFi "instant" loading of configurations direct to the FPGA.
* USB and WiFi updating of the configuration stored in SPIFFS.
* USB and WiFi monitor/control of the FPGA design via SPI.
* USB and WiFi monitor of LiPo battery voltage.
* USB setting of WiFi credentials (SSID and Password)

## Gateware

[![.github/workflows/build-gateware.yml](https://github.com/ICE-V-Wireless/ICE-V-Wireless/actions/workflows/build-gateware.yml/badge.svg)](https://github.com/ICE-V-Wireless/ICE-V-Wireless/actions/workflows/build-gateware.yml)

The [Gateware](Gateware) directory contains HDL source and build materials for
the shipping FPGA demo designs as well as additional designs for learning how to
use the ICE-V-Wireless.

## Host-side Utilities
The [python](python) directory contains the command-line Python scripts which are
provided to communicate over USB and WiFi with the default ICE-V-Wireless firmware
and support the features outlined in the Firmware section above.

## Getting One
ICE-V-Wireless boards are available via the GroupGets store:
https://store.groupgets.com/products/ice-v-wireless

US customers can purchase through Digi-Key:
https://www.digikey.com/en/products/detail/groupgets-llc/ice-v-wireless/17168543

## Getting Started
If you've just gotten an ICE-V-Wireless board and want to quickly get going
read the [Getting Started](Getting-Started.md) page.

## Reviews and Tutorials
* [Tom Verbeure](https://tomverbeure.github.io/) has generously written a great review and tutorial for the
ICE-V-Wireless board on his blog:
[An In-Depth Look at the ICE-V Wireless FPGA Development Board](https://tomverbeure.github.io/2022/12/27/The-ICE-V-Wireless-FPGA-Board.html)

You'll find a lot of additional detail and some clear step-by-step guides
for getting tools set up to build firmware and gateware for the board.

* [gojimmypi](https://gojimmypi.github.io/) has a few writeups on his blog from early-on in the
board development which provide some insight into devtools and build systems plus
providing some helpful links:

  * [ICE-V Wireless FPGA with ESP32-C3](https://gojimmypi.github.io/ICE-V-Wireless-FPGA-ESP32-C3/)

  * [SERV and FuseSoC for the ICE-V Wireless FPGA](https://gojimmypi.github.io/ICE-V-Wireless-SERV-fusesoc/)
