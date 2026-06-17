# QGate Firmware 0.4.1

## Changelog

# 0.4.1
- Fix critique TLS/MQTT : SSLClient validait le certificat du broker contre la date de COMPILATION du firmware (figée), en ignorant l'horloge NTP. À chaque rotation du certificat Let's Encrypt de HiveMQ (tous les 90 j), tout le parc tombait simultanément en « certificate is expired or not yet valid » et ne pouvait plus se connecter. On applique désormais l'heure NTP réelle via `setVerificationTime()` juste après la synchro temps, donc la validation suit l'horloge murale et non la date de build.

# 0.4.0
- CDC (USB virtual COM) scanner support: scanners in USB COM mode are now handled alongside HID keyboard mode, with automatic detection per device — CDC payloads arrive as raw ASCII, bypassing keyboard decoding and layout (FR/EN) handling entirely
- HID frames are now dispatched on the CH559 protocol message type byte: fixes a latent bug where any frame with an 8-byte payload (e.g. an 8-char device string during enumeration) was misparsed as a keyboard report and could inject garbage characters
- Fix scan-end inactivity timer able to fire on the very first character after boot (`lastDecoded` now initialized at `usbHidInit`)
- CH559 module firmware (fork [privronQweekle/CH559sdccUSBHost](https://github.com/privronQweekle/CH559sdccUSBHost)):
  - Fix first HID report lost after USB enumeration (data toggle mismatch) — root cause of the first character missing from the first scan after boot or scanner replug
  - CDC-ACM support: bulk IN polling forwarded over UART as `MSG_TYPE_CDC_DATA` (0x09) frames; line opened with SET_LINE_CODING + SET_CONTROL_LINE_STATE (DTR/RTS)
  - UART output at 115200 baud (matches deployed modules)
  - Requires reflashing the module (see `tools/hid_module_flash/`); old module firmware remains fully compatible with this ESP32 firmware in HID mode
- New tool `tools/hid_sniffer/`: minimal UART sniffer firmware (per-byte timestamps, RAM-buffered) + `analyze.py` capture/analysis script that decodes HID and CDC frames, detects USB enumerations and diffs scans against an expected barcode
- `tools/hid_module_flash/` cleaned up: single up-to-date `CH559USB.bin` (data toggle fix + 115200 + CDC), rewritten flash instructions, stale test binaries and leftover PlatformIO skeleton removed

# 0.3.0
- Fix first character of barcode dropped after scanner idle: extended timeout to 2s for short fragments (<4 chars) to handle scanner wake-up latency, with 150ms timeout preserved for normal scans
- Drop spurious scan fragments shorter than 4 characters instead of publishing them as incomplete scans
- HID debug: reworked `hid/scan` MQTT dump to JSON format (`{"type":"raw","seq":N,"total":N,"hex":"..."}`) with multi-part reassembly support; fragment rejections published as `{"type":"fragment",...}`
- HID debug: key-up frames excluded from hex dump (halves dump size); all debug overhead skipped entirely when debug is disabled
- Updated `tools/mqttfx_clean_hex.py` to handle both `hid/frame` (plain hex) and `hid/scan` (JSON) topic formats, with MQTT.fx noise filtering and multi-part reassembly
- GitHub Pages: deploy `dev` branch to `/dev/` subfolder alongside main site
- Fix first character lost on consecutive scans: `finishScan()` no longer clears `rawBuf` mid-loop, preventing frames already buffered for the next scan from being discarded
- Fix provisioning ignoring re-provisioning on already-provisioned devices: removed guard that silently dropped `CFGTESTA` QR codes when credentials were already set
- Fix `mastersuffix` not stored when re-provisioning: refactored provisioning payload application into shared `applyProvisioningPayload()` used by both QR and MQTT flows
- New MQTT endpoint `qgate/{id}/provision`: accepts `{"provision":"<base64>"}` (PSK-encrypted `user:password:mastersuffix`) for direct provisioning without QR code handshake
- Heartbeat includes `master_suffix` field when debug is active and a provisioning suffix is set
- New tool `tools/decrypt_provisioning.py`: decrypts and parses a provisioning base64 payload for verification
- Heartbeat now includes `last_scan_ms` (total time from first char to relay command received) once after each accepted scan, then cleared
- Heartbeat MQTT output handler logs `mqtt_output_rtt_ms` and `total_ms` timings on each relay command received
- Heartbeat JSON built with ArduinoJson instead of snprintf to avoid buffer sizing issues

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
