# QGate Firmware 0.2.1

## Changelog

# 0.2.1
- Fix OTA download failing due to TLS record truncation: enabled `ETHERNET_LARGE_BUFFERS` with `MAX_SOCK_NUM=2` to increase W5500 socket RX buffer from 2 KB to 8 KB
- Fix manifest JSON parser buffer too small (512 → 1024 bytes) which could silently truncate the manifest body
- Remove shared `qgate/ota` broadcast topic — OTA updates now target individual devices via `qgate/{device_id}/ota` only
- Daily automatic reset at 04:00 UTC

# 0.2.0
- Boot OTA: check GitHub Pages manifest at startup and auto-update if a newer version is available
- HTTP fallback if NTP fails or TLS certificate is expired (manifest and firmware fetched over HTTP)
- HMAC-SHA256 manifest signature (PROV_PSK) verified before any flash
- Firmware SHA256 added to manifest.json, computed automatically by the release script
- Boot OTA firmware URL restricted to qweekleservices.github.io (domain check on manifest-provided URL)
- OTA download timeouts: 2 min global, 10 s stall
- Native anti-rollback: versions older than the running firmware are always rejected

# 0.1.4
- Patch NTP provoking certificate renewal failure.

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
