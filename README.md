# qgate-esp32-flash

Flash QGate ESP32 firmware directly from your browser:
**[https://qweekleservices.github.io/qgate-esp32-flash](https://qweekleservices.github.io/qgate-esp32-flash)**

---

## OTA MQTT message — v0.1.2

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.1.2",
  "url": "https://qweekleservices.github.io/qgate-esp32-flash/firmware.bin",
  "sha256": "d17c13f0b923e334a7852659355495f07c84bd7f58e2683163590d9c6979b57f"
}
```

Or target a single device on `qgate/{device_id}/ota`.
