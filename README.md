# Inter Sensores — public firmware repository

Public files used by Inter Sensores BLE freezer-monitor gateways (ESP32 and
ESP32-C3). They are served from `raw.githubusercontent.com`; the gateways and the
installer page verify every one of them before using it.

| Path | What |
|---|---|
| `fw/gateway-v<ver>.bin`, `fw/gateway-c3-v<ver>.bin` | **OTA binaries** (app only). Signed with the firmware key (ECDSA P-256); a gateway only installs an image whose SHA-256 and signature match the per-gateway config. |
| `fw/gateway-v<ver>-full.bin`, `fw/gateway-c3-v<ver>-full.bin` | **Full install images** for a new device: bootloader + partition table + boot_app0 + app, written at `0x0`. Signed with the same key. The app part is byte-identical to the OTA binary of the same version. |
| `directory.gwd` | **Signed directory**: tells the gateways where the server is (address, TLS pins, server key). Rejected unless signed with the config key and not older than the one already accepted. |
| `cfg/` | **Sealed configs** (legacy, firmware ≤ 1.8): encrypted for one gateway each and signed. |

**Nothing secret is stored here.** Binaries contain only public keys; configs are
encrypted for each device; any file changed without the right signature is
refused by the devices and by the installer page.

## Installing a NEW gateway

Normal path: the Inter Sensores panel → **Install firmware** (admins).
(Portuguese step-by-step guide for testers: [INSTALAR.md](INSTALAR.md).)

1. Use **Chrome or Edge on a desktop computer** (Windows, macOS, Linux,
   ChromeOS). Phones, Firefox and Safari cannot talk to USB serial devices.
2. Plug the board in with a USB **data** cable (charge-only cables do not work).
   On Windows, if no port shows up, install the USB driver of the board's
   USB chip: **CP210x** (Silicon Labs) or **CH340** (WCH). The **ESP32-C3
   SuperMini** uses the chip's native USB (no driver on Windows 10+); if the
   browser does not find it, hold BOOT while plugging the cable in.
3. Pick the board (ESP32 DevKit or ESP32-C3 SuperMini) and click **Download and verify**: the page
   checks size, SHA-256 and the signature of the image.
4. Click **Connect and install**, choose the port. The chip is **erased
   completely** and the image is written. If it cannot connect, hold **BOOT**,
   press and release **EN/RST**, release **BOOT** and try again.
5. After the reset the device is a brand-new gateway (new identity). Hold BOOT
   for ~5 s to open its setup network `InterSensores-XXXX`, connect a phone,
   open the portal (http://192.168.4.1), set WiFi, name and sensors, and enter
   an activation code created in the panel.
6. From then on the gateway updates itself over the air (OTA) whenever a newer
   signed version is published.

### Command-line alternative (esptool)

```bash
esptool.py --chip esp32 erase_flash
esptool.py --chip esp32 write_flash 0x0 gateway-v1.10-full.bin
# ESP32-C3: --chip esp32c3 and gateway-c3-v<ver>-full.bin
```

Erasing first matters: it wipes any previous identity, WiFi and queue, exactly
like the panel installer. Check the file's SHA-256 against the value shown by
the panel before flashing.
