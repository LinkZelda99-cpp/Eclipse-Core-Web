# ECLIPSE CORE WEB

A lightweight, mobile-first local web interface for the Arduino UNO R4 WiFi edition of ECLIPSE CORE.

## Current V1 features

- Device connection, IP address, application state, and uptime
- Cached light, distance, temperature, and humidity readings
- Remote menu navigation: up, down, select, and back
- Responsive dark interface
- No cloud account, hosted backend, or third-party runtime dependency

## Connect your phone

1. Configure the Wi-Fi credentials in the firmware repository's local `arduino_secrets.h` file.
2. Upload the ECLIPSE CORE integration branch to an Arduino UNO R4 WiFi.
3. Keep your phone on the same Wi-Fi network as the Arduino.
4. Open the local IP address shown in CORE INFO on the device's LCD.

The Arduino serves both the page and its API. Do not use GitHub Pages as the device-control host: serving from the Arduino keeps requests same-origin and the interface local to your network.

## Source and firmware bundle

Editable source:

- `index.html`
- `assets/app.css`
- `assets/app.js`

The firmware repository contains generated `CoreWebPage.h` so the UNO R4 WiFi can serve the page without an SD card or external server.

After changing the website source, regenerate the firmware header:

```bash
python scripts/build_arduino_header.py ../ECLIPSE-CORE/CoreWebPage.h
```

Adjust the relative path if the repositories are not sibling folders.

## API contract

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/v1/status` | Device, app, uptime, cached sensor values |
| POST | `/api/v1/input/up` | Move menu selection up |
| POST | `/api/v1/input/down` | Move menu selection down |
| POST | `/api/v1/input/select` | Activate current selection |
| POST | `/api/v1/input/back` | Trigger the existing back-button path |

Commands are injected into the same input events used by the physical controls. The website does not create a separate application-control system.

## Scope and limitations

V1 is the first integration milestone. It does not yet provide matrix drawing, RGB color mixing, song selection, game-specific controls, or remote settings editing. Sensor values are the most recently cached readings from the firmware applications; the web server does not independently sample sensors.

CORE continues running its local interface when Wi-Fi is unavailable. Network startup is optional and non-blocking. Current control endpoints are intended for a trusted local network; add authenticated pairing before exposing any broader control surface.
