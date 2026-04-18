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
  "sha256": "45200d23b19f73268efcf77b505ec90493e065d43ab0e413eda7b60eceacd1e8"
}
```

Or target a single device on `qgate/{device_id}/ota`.
