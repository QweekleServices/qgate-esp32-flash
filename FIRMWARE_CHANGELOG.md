# QGate Firmware 0.2.3-a

## Changelog

# 0.2.3
- Fix first character of barcode dropped after scanner idle: extended timeout to 2s for short fragments (<4 chars) to handle scanner wake-up latency, with 150ms timeout preserved for normal scans
- Drop spurious scan fragments shorter than 4 characters instead of publishing them as incomplete scans
- HID debug: reworked `hid/scan` MQTT dump to JSON format (`{"type":"raw","seq":N,"total":N,"hex":"..."}`) with multi-part reassembly support; fragment rejections published as `{"type":"fragment",...}`
- HID debug: key-up frames excluded from hex dump (halves dump size); all debug overhead skipped entirely when debug is disabled
- Updated `tools/mqttfx_clean_hex.py` to handle both `hid/frame` (plain hex) and `hid/scan` (JSON) topic formats, with MQTT.fx noise filtering and multi-part reassembly
- GitHub Pages: deploy `dev` branch to `/dev/` subfolder alongside main site

# 0.2.2
- Robust connection state machine: NO_LINK → DHCP_WAIT → NTP_SYNC → MQTT_CONNECT → ONLINE with LED/buzzer feedback at each stage
- MQTT exponential backoff (5s → 10s → 30s → 60s cap) with fallback to default credentials after 5 consecutive failures
- Fix SSL write error spam after cable unplug: `mqttDisconnect()` now fully stops and clears the TLS client, and all MQTT API calls use a shadow connected state to avoid touching SSLClient when known-disconnected
- Extract connection management to `connection.cpp` / `connection.h`; `main.cpp` reduced to ~390 lines
- Heartbeat enriched with `state` and `ip` fields for remote debugging

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
