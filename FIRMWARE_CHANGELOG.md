# QGate Firmware 0.1.2

## OTA MQTT message

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.1.2",
  "url": "https://raw.githubusercontent.com/privronQweekle/qgate-esp32-flash/main/firmware.bin",
  "sha256": "823275a12670d4a8fbb07d2c342f48e3d95d619b2671646f678e26e0a15b9c54"
}
```

Or target a single device on `qgate/{device_id}/ota`.

---

## Changelog

# 0.1.2
- Add drop of partial frame of HID

# 0.1.1
- Add debug of HID through MQTT
- Add support for Oxhoo BC232 scanner
