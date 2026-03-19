# QGate Firmware 0.1.3

## OTA MQTT message

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.1.3",
  "url": "https://qweekleservices.github.io/qgate-esp32-flash/firmware.bin",
  "sha256": "d17c13f0b923e334a7852659355495f07c84bd7f58e2683163590d9c6979b57f"
}
```

Or target a single device on `qgate/{device_id}/ota`.

---

## Changelog

# 0.1.3
- Nouveau master pass
- Rework de la procédure de pairing pour ne plus scanner le mdp
- Rework de la gestion du HID pour éviter les pertes de chars
- Buzzer lorsque le scanner est pret
- Rework de la gestion du switch de layout pour ne plus double scanner
- Support mise à jour depuis github
- Use longer ID
- Add max duration of relay open for 1h
- For master code check only start with so afterprovision master pass can be used before as well

# 0.1.2
- Add drop of partial frame of HID

# 0.1.1
- Add debug of HID through MQTT
- Add support for Oxhoo BC232 scanner
