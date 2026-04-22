# qgate-esp32-flash

Flash QGate ESP32 firmware directly from your browser:
**[https://qweekleservices.github.io/qgate-esp32-flash](https://qweekleservices.github.io/qgate-esp32-flash)**

---

## OTA MQTT message — v0.1.4

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.1.4",
  "url": "https://qweekleservices.github.io/qgate-esp32-flash/firmware.bin",
  "sha256": "ed6e0ed8f80db489be18b61349ce3ab67cdbf0fe4efa056a021be0d8a1015c1e"
}
```

Or target a single device on `qgate/{device_id}/ota`.
