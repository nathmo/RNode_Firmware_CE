# Building
## Prerequisites
The build system of this repository is based on GNU Make. The `Makefile` is in the base of the repository. Please ensure you have `arduino-cli`, `python3` and `make` installed before proceeding.

Firstly, figure out which MCU platform your supported board is based on. The table below can help you.

| Board name | Link | Transceiver | MCU | Description | 
| :--- | :---: | :---: | :---: | :---: |
| Handheld v2.x RNodes | [Buy here](https://unsigned.io/shop/product/handheld-rnode) | SX1276 | ESP32 |
| RAK4631 | [Buy here](https://store.rakwireless.com/products/rak4631-lpwan-node?m=5&h=wisblock-core) | SX1262 | nRF52 |
| LilyGO LoRa32 v1.0 | [Buy here](https://www.lilygo.cc/products/lora32-v1-0) | SX1276/8 | ESP32 |
| LilyGO T-BEAM v1.1 | [Buy here](https://www.lilygo.cc/products/t-beam-v1-1-esp32-lora-module) | SX1276/8 | ESP32 |
| LilyGO LoRa32 v2.0 | No link | SX1276/8 | ESP32 | Discontinued? |
| LilyGO LoRa32 v2.1 |  [Buy here](https://www.lilygo.cc/products/lora3) | SX1276/8 | ESP32 | With and without TCXO |
| Heltec LoRa32 v2 | No link | SX1276/8 | ESP32 | Discontinued? |
| Heltec LoRa32 v3 | [Buy here](https://heltec.org/project/wifi-lora-32-v3/) | SX1262 | ESP32 | 
| Homebrew ESP32 boards | | Any supported | ESP32 | This can be any board with an Adafruit Feather (or generic) ESP32 chip |

## Setup
on linux you can do the following to either compile for an existing board or as a starting point to create your own.
```
sudo apt update
sudo apt install git python3 python3-pip make
curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh
```
once you have the required dependency you can add the arduino-cli to the path so that the shell knows its a command
```
nano ~/.bashrc
```
and add this line at the end (change your used if it's not ubuntu) and check that the path correspond to what the arduino-cli install script returned.
```
export PATH=$PATH:/home/ubuntu/.local/bin
```
save and exit then reboot
```
sudo reboot now
```
then you can clone this repo in a location and your choice
```
git clone https://github.com/liberatedsystems/RNode_Firmware_CE.git
cd RNode_Firmware_CE
```
now we create a virtual environnement that MUST be reactivated whenver you resume working with this project
to create it : 
```
python -m venv venv
```
just run the following command to reactivate it every time
```
source venv/bin/activate
```
and this to deactivate it once you are done (not now)
```
deactivate # DO NOT RUN ME NOW. ONLY WHEN YOU ARE DONE.
```

now you can install the required python utilities
```
pip install pyserial rns
```
and install the required dependency to build :
```
make prep
```
alternatively you can run `make prep-esp32` or` make prep-nrf` if you know what platform you are building from to save up some time.

you are now ready to compile the firmware.


## Compiling
Next, you need to find the name of the target for your board. Please reference the table below to do so:
| Board name | Target | 
| :--- | :---: |
| Handheld v2.x RNodes | `rnode_ng_20` |
| RAK4631 | `rak4631` |
| LilyGO T-BEAM v1.1 | `tbeam` |
| LilyGO T-BEAM v1.1 (SX1262) | `tbeam_sx126x` |
| LilyGO LoRa32 v1.0 | `lora32_v10` |
| LilyGO LoRa32 v2.0 | `lora32_v20` |
| LilyGO LoRa32 v2.1 | `lora32_v21` |
| Heltec LoRa32 v2 | `heltec32_v2` |
| Heltec LoRa32 v3 | `heltec32_v3` | 
| Homebrew ESP32 boards | `genericesp32` |

After you've ascertained the target for the board simply run the following to compile for the board:

`make firmware-[target]`

Ensure you replace [target] with the target you selected. For example:

`make firmware-rak4631`

## Flashing
To flash the chosen board (if you have one), simply connect it to your computer over USB, and then run the following:

`make upload-[target]`

Ensure you replace [target] with the target you selected. For example:

`make upload-rak4631`

**Please note**, you must re-compile the firmware each time you make changes **before** you flash it, else you will just be flashing the previous version of the firmware without the new changes!

These commands can also be run as a one liner. For example:

`make firmware-[target] && make upload-[target]`

This is especially helpful when making continuous changes to the firmware and testing them out.
