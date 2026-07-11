# WebAugur

**WebAugur** is a fork of [ESP32 Bit Pirate](https://github.com/geo-tp/ESP32-Bit-Pirate) — open-source firmware that turns an ESP32 into a multi-protocol development and analysis tool, inspired by the legendary Bus Pirate.

This fork lives under [webaugur/WebAugur](https://github.com/webaugur/WebAugur). Upstream remains the source of truth for many docs and tools until WebAugur-specific infrastructure catches up.

It supports sniffing, sending, scripting, and interacting with digital protocols (I2C, UART, 1-Wire, SPI, and more) via a serial terminal or web-based CLI, plus radio protocols such as Bluetooth, Wi-Fi, Sub-GHz, and RFID.

![Demo showing the different modes of the firmware](images/bit_pirate_uart.gif)
![Demo showing the LittleFS file system](images/pirate_assistant.gif)

## Fork relationship

| | URL |
|---|---|
| **This fork** | https://github.com/webaugur/WebAugur |
| **Upstream** | https://github.com/geo-tp/ESP32-Bit-Pirate |
| **Upstream website** | https://geo-tp.github.io/ESP32-Bit-Pirate/ |
| **Upstream wiki** | https://github.com/geo-tp/ESP32-Bit-Pirate/wiki |

Upstream website, web flasher, web tools, and wiki still apply and are linked below. WebAugur branding is applied in the firmware UI, web terminal title, and this repository.

## Features

- Interactive command-line interface (CLI) via **USB Serial or WiFi Web**.
- **Modes for:**
   - [HiZ](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/01-HiZ) (default)
   - [I2C](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/05-I2C) (scan, glitch, slave mode, dump, eeprom)
   - [SPI](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/06-SPI) (eeprom, flash, sdcard, slave mode)
   - [UART](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/03-UART) / [Half-Duplex UART](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/04-HDUART) (bridge, read, write)
   - [1WIRE](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/02-1WIRE) (ibutton, eeprom)
   - [2WIRE](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/07-2WIRE) (sniff, smartcard) / [3WIRE](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/08-3WIRE) (eeprom)
   - [DIO](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/09-DIO) (Digital I/O, read, pullup, set, pwm)
   - [Infrared](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/11-INFRARED) (send, record, universal remote)
   - [USB](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/12-USB) (HID, flashrom, storage, usb-uart)
   - [Bluetooth](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/13-BLUETOOTH) (BLE HID, scan, spoofing, sniffing)
   - [Wi-Fi](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/14-WIFI) / [Ethernet](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/18-ETHERNET) (sniff, deauth, nmap, netcat)
   - [JTAG](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/15-JTAG) (scan, SWD, openOCD)
   - [LED](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/10-LED) (animations, set LEDs)
   - [I2S](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/16-I2S) (test speakers, mic, play sound)
   - [CAN](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/17-CAN) (sniff, send and receive frames)
   - [SUBGHZ](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/19-SUBGHZ) (analyze, record, replay)
   - [RFID](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/20-RFID) (read, write, clone)
   - [RF24](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/21-RF24) (scan, send, receive)
   - [FM](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/22-FM) (analyze, broadcast)
   - [CELL](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/23-CELL) (dump sim card, sms, call)

- **Protocol sniffers** I2C, UART, SPI, 1Wire, 2wire, CAN, Wi-Fi, Bluetooth, SubGhz.
- Baudrate **auto-detection**, AT commands and various tools for UART.
- Registers manipulation, **EEPROM dump tools**, identify devices for I2C.
- Read all sort of **EEPROM, Flash** and various others tools for SPI.
- Scripting using **Bus Pirate-style bytecode** instructions or **Python**.
- Device-B-Gone command with more than **80 supported INFRARED protocols**.
- Direct I/O management, **PWM, servo, GPIOs state**.
- Analyze radio signals and frequencies **on every bands**.
- Near than **50 addressable LEDs protocols** supported.
- **Ethernet and WiFi** are supported to access networks.
- Import and export data with the **LittleFS over HTTP.**
- **WebAugur Assistant** (web UI AI helper) to help you with the firmware.
- **USB-Uart dongle, SPI programmer, logic analyzer** and more.
- [**Web Serial tools**](https://geo-tp.github.io/ESP32-Bit-Pirate/web-tools/) to use USB Serial over a web browser.

## Supported Devices


| Device               |                                     | Description                       |
|-----------------------|------------------------------------------|---------------------------------------------------|
| **ESP32 S3 Dev Kit**  | ![Photo of the ESP32 S3 Dev Kit](/images/s3-devkit_s.jpg)     | More than 20 available GPIO, 1 button |
| **LILYGO T-Display** | ![Photo of the T-Display-S3](/images/t_displays3_s.jpg) | 13 GPIO (1 Qwicc), screen, 2 buttons |
| **LILYGO T-Embed**    | ![Photo of the LILYGO T-Embed](/images/tembed_s.jpg)          | 9 GPIO (Grove, Header), screen, encoder, speaker, mic, SD card                                         |
| **LILYGO T-Embed CC1101** | ![Photo of the LILYGO T-Embed CC1101](/images/tembedcc1101_s.jpg) | 4 GPIO (2x Qwiic), screen, encoder, speaker, mic, SD Card, CC1101, PN532, IR TX, IR RX , battery                                 |
| **LILYGO T-Embed CC1101 Plus** | ![Photo of the LILYGO T-Embed CC1101 Plus](/images/tembedcc1101_s.jpg) | 4 GPIO (2x Qwiic), screen, encoder, speaker, mic, SD Card, CC1101, NRF24, PN532, IR TX, IR RX , battery                                 |
| **M5 AtomS3 Lite**    | ![Photo of the M5 Atom S3 Lite](/images/atom_s.jpg)            | 8 GPIO (Grove, Header), IR TX, 1 buttton                  |
| **M5 Cardputer**      | ![Photo of the M5 Cardputer](/images/cardputer_s.png)            | 2 GPIO (Grove), screen, keyboard, mic, speaker, IR TX, SD card, battery, [standalone mode](#standalone-mode-for-the-cardputer)            |
| **M5 Cardputer ADV**  | ![Photo of the M5 Cardputer ADV](/images/cardputer-adv_s.jpg)    | 12 GPIO (Grove, Header), screen, keyboard, mic, speaker, IR TX, SD card, IMU, battery, [standalone mode](#standalone-mode-for-the-cardputer)                  |
| **M5 StampS3**        | ![Photo of the M5 StampS3](/images/stamps3_s.jpg)             | 9 GPIO (exposed pins), 1 button                       |
| **M5 Stick S3** | ![Photo of the M5 Stick S3](/images/m5sticks3_s.jpg)      | 13 GPIO (Grove, Header), screen, mic, speaker, IR TX, IR RX, IMU, 3 buttons, battery                 |
| **Seeed Studio Xiao S3** | ![Photo of the Seeed Studio Xiao ESP32-S3](/images/xiaos3_s.jpg)        | 9 GPIO (exposed pins), 1 button |

- **Other ESP32-S3-based Boards**

  - All boards based on the **ESP32-S3 can be supported**, provided they have at least **8 MB of flash.**

  - You can **flash the s3 dev-kit firmware onto any ESP32-S3 board.**

  - Keep in mind that the **default pin mapping in the firmware may not match** your specific board.

## Getting Started

[![Banner of the web flasher](images/flasher.jpg)](https://geo-tp.github.io/ESP32-Bit-Pirate/webflasher/)

1. 🔧 Flash the firmware
   - Build and upload this fork with PlatformIO (see [Build](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Build)), or use the [upstream Web Flasher](https://geo-tp.github.io/ESP32-Bit-Pirate/webflasher/) for stock Bit Pirate builds.
   - You can also burn stock builds on [M5Burner](https://docs.m5stack.com/en/download), in the StickS3, AtomS3, M5StampS3 or Cardputer category.

2. 🔌 Connect via Serial or Web
   - Serial: any terminal app, or the [free browser-based Web Serial terminal](https://geo-tp.github.io/ESP32-Bit-Pirate/web-tools/web-serial-terminal/) (see [Connect via Serial](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Serial))
   - Web: configure Wi-Fi and access the CLI via browser (see [Wi-Fi Connection](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/00-Terminal))

3. 🧪 Use commands like:
   ```
   mode
   help
   scan
   sniff
   ```

## Documentation

[![Banner of the wiki page](images/bus_pirate_wiki.png)](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/)

📚 **[Visit the upstream Wiki](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki)** for detailed documentation on every mode and command.

- [Terminal mode](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/00-Terminal) - About serial and web terminal.
- [Mode overviews](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki) - Browse supported modes.
- [Serial setup](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Serial) - Serial access via USB.

## Python Scripts

[![Banner of the Scripts page](images/bus_pirate_scripts.png)](https://github.com/geo-tp/ESP32-Bit-Pirate-Scripts/)

🛠️ [**Automate interactions over serial**](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Python) using **Python scripts.**

You can write and test scripts directly in the browser with the [Python Lab](https://geo-tp.github.io/ESP32-Bit-Pirate/web-tools/python-lab/).

## Expander

[![Banner of the Expander page](images/bus_pirate_exp.png)](https://github.com/geo-tp/ESP32-Bus-Expander)

🔌 **[Expand capabilities](https://github.com/geo-tp/ESP32-Bus-Expander)** with additional hardware modules.

## Dock

[![Banner of the Dock page](images/bus_pirate_dock.png)](https://github.com/AndreiVladescu/ESP32-Bit-Pirate-Dock)

🔧 **[A docking station for the ESP32 S3 DevKit](https://github.com/AndreiVladescu/ESP32-Bit-Pirate-Dock) designed to work with original Bus Pirate adapters.**
It allows you to plug and use the original [Bus Pirate](https://buspirate.com/) ecosystem of adapters and accessories.

![The dock board](images/bus_pirate_dock_board.png)

## CLI Interfaces

The firmware provides three command-line interface (CLI) modes:

| Interface         | Advantages                                                                 | Ideal for...                          |
|------------------|-----------------------------------------------------------------------------|----------------------------------------|
| **Web Interface** | - Accessible from any browser<br>- PC, tablets, mobiles<br>- Works over Wi-Fi<br>- No cables needed | Quick tests, demos, headless setups   |
| **Serial Interface** | - Faster performance<br>- Instant responsiveness<br>- Handles large data smoothly | Intensive sessions, frequent interactions |
| **Standalone** | - Only for the Cardputer<br>- On device keyboard<br>- On device screen | Portable sessions, Quick tests |


All interfaces share the same command structure and can be used interchangeably ([more details](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/00-Terminal)).

## Mobile Web Interface over WiFi
![An iPhone screenshot showing the firmware web interface](images/presentation_mobile.png)

## Standalone Mode for the Cardputer
![A Cardputer running in standalone mode](images/standalonemode_s.png)

## Browser-Based Web Serial Tools

The [upstream Web Serial Tools](https://geo-tp.github.io/ESP32-Bit-Pirate/web-tools/) provide direct access to the Serial CLI from a compatible browser, without installing PuTTY, minicom, or another terminal application.

![A demo using Web Serial Tools](images/web_tools_demo.gif)

## Contribute

See [How To Contribute](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Contribute) and [CONTRIBUTING.md](CONTRIBUTING.md) for adding a new command to any mode.

Pull requests for WebAugur-specific work should target this repository: https://github.com/webaugur/WebAugur

## Develop / sync with upstream

```bash
# remotes (already configured in this workspace)
# origin   -> webaugur/WebAugur
# upstream -> geo-tp/ESP32-Bit-Pirate

git fetch upstream
git merge upstream/pioarduino   # or rebase, depending on preference
```

Default branch: **`pioarduino`** (PlatformIO + Arduino / pioarduino platform).

## Visual Assets

#### [![Small logo](images/logo_square_small.png)](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Visual-Assets)

See [images, logo, presentations, photo, video, illustrations](https://github.com/geo-tp/ESP32-Bit-Pirate/wiki/99-Visual-Assets). Upstream visuals can be **freely used in blog posts, documentation, videos, or articles**. WebAugur-specific branding can be added over time.

## Warning
> ⚠️ **Voltage Warning**: Devices should only operate at **3.3V** or **5V**.
> - Do **not** connect peripherals using other voltage levels — doing so may **damage your ESP32**.

> ⚠️ **Usage Warning**: This firmware is provided for **educational, diagnostic, and interoperability testing purposes only**.
> - Do not use it to interfere with, probe, or manipulate devices without proper authorization.
> - Avoid any unauthorized RF transmissions (e.g., sub-GHz) that could violate local regulations or disrupt networks and communications.
> - The authors are not responsible for any misuse of this software or hardware, including legal consequences resulting from unauthorized access or signal emission.
> - Always stay within the bounds of your country’s laws and responsible disclosure policies.

## License

MIT — same as upstream. Copyright notices of original authors are retained. See [LICENSE](LICENSE).
