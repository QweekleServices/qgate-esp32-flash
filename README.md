# qgate-esp32-flash

Flash QGate ESP32 firmware directly from your browser:
**[https://qweekleservices.github.io/qgate-esp32-flash](https://qweekleservices.github.io/qgate-esp32-flash)**

---

## OTA MQTT message — v0.2.0

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.2.0",
  "url": "https://qweekleservices.github.io/qgate-esp32-flash/firmware.bin",
  "sha256": "bd82203110b1b848f43b33e24fc8bdf0353b2110faaa0f4a5fc7516b7d6ecfec",
  "signature": "91cc6280fae038f85812140a52868d1ca5ed1198f3f1130d5fedce209b854dab"
}
```

Or target a single device on `qgate/{device_id}/ota`.
