# QGate Firmware 0.1.3

## OTA MQTT message

Publish this payload on topic `qgate/ota` with **retain=true** to trigger an OTA update on all devices:

```json
{
  "v": "0.1.3",
  "url": "https://raw.githubusercontent.com/privronQweekle/qgate-esp32-flash/main/firmware.bin",
  "sha256": "68534d9f5544fe85aa3d047d9fe6b145b02a973531c56d615fbdd1067be52b7d"
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

# 0.1.2
- Add drop of partial frame of HID

# 0.1.1
- Add debug of HID through MQTT
- Add support for Oxhoo BC232 scanner
