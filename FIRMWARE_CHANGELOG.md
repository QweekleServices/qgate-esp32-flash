# QGate Firmware 0.1.4

## Changelog

# 0.1.4
- fix: sync NTP via EthernetUDP (NTPClient) au lieu de configTime()
  - configTime() passait par lwIP alors que arduino-libraries/Ethernet (W5500 SPI) bypass lwIP
  - time(nullptr) restait à 0 → BearSSL rejetait tous les certs TLS comme "not yet valid"
  - Bug révélé par le renouvellement du cert HiveMQ Cloud du 2026-04-17

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
