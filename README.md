<p align="center">
  <img src="mishmesh/docs/img/logo.png" alt="mishmesh" width="420">
</p>

<p align="center"><b>Phone-optional MeshCore companion firmware with a full on-device UI.</b></p>

<p align="center">
  <a href='https://ko-fi.com/W3V222VPDT' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://storage.ko-fi.com/cdn/kofi6.png?v=6' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
</p>

`mishmesh` is a fork of [MeshCore](https://github.com/meshcore-dev/MeshCore) focused on standalone operation.  
Instead of relying on a paired phone for core tasks, it puts messaging, contacts, repeater tools, and settings directly on the radio.

## What this firmware adds

- Full local messaging workflow (DMs, channels, rooms)
- Contact management and path controls
- Repeater login + management from device
- On-device clock tools (stopwatch/timer/alarm/world clock)
- Airtime and duty-cycle visibility
- First-boot onboarding and local settings

## Current hardware target

Primary target in this repository: **Wio Tracker L1 (Pro)**.

ThinkNode M5 companion environments are included and documented below.

## Screens

<table>
  <tr>
    <td align="center"><img src="mishmesh/docs/img/home.png" width="240"><br><sub>Home</sub></td>
    <td align="center"><img src="mishmesh/docs/img/appmenu.png" width="240"><br><sub>App menu</sub></td>
    <td align="center"><img src="mishmesh/docs/img/contacts.png" width="240"><br><sub>Contacts</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="mishmesh/docs/img/contact_detail.png" width="240"><br><sub>Contact detail</sub></td>
    <td align="center"><img src="mishmesh/docs/img/chat.png" width="240"><br><sub>Conversation</sub></td>
    <td align="center"><img src="mishmesh/docs/img/quickreplies.png" width="240"><br><sub>Quick replies</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="mishmesh/docs/img/keypad.png" width="240"><br><sub>Keypad</sub></td>
    <td align="center"><img src="mishmesh/docs/img/repeater.png" width="240"><br><sub>Repeater status</sub></td>
    <td align="center"><img src="mishmesh/docs/img/settings.png" width="240"><br><sub>Settings</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="mishmesh/docs/img/clock_stopwatch.png" width="240"><br><sub>Stopwatch</sub></td>
    <td align="center"><img src="mishmesh/docs/img/clock_timer.png" width="240"><br><sub>Timer</sub></td>
    <td align="center"><img src="mishmesh/docs/img/clock_alarm.png" width="240"><br><sub>Alarm</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="mishmesh/docs/img/clock_world.png" width="240"><br><sub>World clock</sub></td>
    <td></td>
    <td></td>
  </tr>
</table>

## Install (Wio Tracker L1)

Download the latest `WioTrackerL1_companion_radio_*_mishmesh-*.uf2` from [Releases](../../releases), then:

1. Connect the Wio Tracker L1 over USB.
2. Double-tap reset to mount the UF2 drive.
3. Copy the `.uf2` file to the drive.

Use `*_ble` for Bluetooth companion mode or `*_usb` for USB-serial companion mode.

## ThinkNode M5

### ThinkNode M5 hardware specs

Compiled from the Meshtastic device catalog for ThinkNode M5 / Elecrow:

- **MCU:** ESP32-S3 (Wi-Fi 2.4 GHz b/g/n + BLE 5)
- **LoRa radio:** Semtech SX1262
- **Display:** 1.54" E-Ink
- **GNSS:** GPS / GLONASS / BeiDou / QZSS
- **Battery:** 1200 mAh rechargeable Li-ion
- **Connector:** USB-C
- **Band variants:** US 902–928 MHz or EU 868 MHz

Reference: Meshtastic hardware docs, ThinkNode series page (which links Elecrow’s official wiki).

### ThinkNode M5 buttons in this firmware

ThinkNode M5 has **two physical buttons** mapped by this repo:

- `PIN_USER_BTN` (GPIO 21): primary UI input
- `PIN_BUTTON2` (GPIO 14): secondary button used as `BACKLIGHT_BTN`

Current `ui-new` behavior:

- **Primary button single click:** next page
- **Primary button double click:** previous page
- **Primary button long press:** enter / page action
- **Primary button triple click:** select action
- **Secondary button:** backlight control path (`BACKLIGHT_BTN`), separate from page navigation

Page-specific primary-button actions:

- **Node page:** long press sends advert
- **Settings page:**
  - long press toggles BLE/serial
  - triple click toggles GPS
- **Map page:**
  - triple click clears track breadcrumbs
  - long press cycles map mode label
- **Power page:** long press hibernates

### Flashing ThinkNode M5 from this repo

```sh
export FIRMWARE_VERSION=mishmesh-dev
pio run -e ThinkNode_M5_companion_radio_ble -t upload
# or
pio run -e ThinkNode_M5_companion_radio_usb -t upload
```

If upload does not begin, enter ROM bootloader mode (hold **BOOT**, tap **RESET**, release **BOOT**) and retry.

## Build from source

```sh
export FIRMWARE_VERSION=mishmesh-dev
pio run -e WioTrackerL1_companion_radio_usb_mishmesh -t upload
# or WioTrackerL1_companion_radio_ble_mishmesh
```

Package release artifacts to `out/`:

```sh
export FIRMWARE_VERSION=mishmesh-dev
sh build.sh build-firmware WioTrackerL1_companion_radio_usb_mishmesh
```

## Emoji

Emoji rendering uses the licensed [EmojiMania](https://idanro.itch.io/emojimania) glyph set.  
This repository does not redistribute those glyph assets. Official release builds include them; local/fork builds work normally but render placeholder blocks unless you provide your own licensed sheet.

See: [`mishmesh/text/emoji-tools/README.md`](./mishmesh/text/emoji-tools/README.md)

## Architecture notes

- UI framework: [`mishmesh/`](./mishmesh)
- Companion bridge adapter: [`examples/companion_radio/ui-mishmesh/`](./examples/companion_radio/ui-mishmesh)
- Screen model: `Applet` subclasses managed by `AppletHost`
- Rendering: `Canvas` + bitmap font pipeline

## About MeshCore

This project keeps the MeshCore foundation and protocol model intact, while adding a standalone-first UX layer.

Upstream MeshCore: **https://github.com/meshcore-dev/MeshCore**

## License

MIT (same as MeshCore).
