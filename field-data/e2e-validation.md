# HeMB E2E Validation — Production Hardware

## Date: 2026-03-30

## Hardware
- **mule01**: Raspberry Pi 5 8GB, ARM64, Meshtastic Heltec V4 + Iridium 9603N
- **rocket01**: x86_64 mini-PC, Meshtastic T-Echo + RockBLOCK 9704 IMT

## Bond Group
- `bond_field_01`: mesh_0 (Meshtastic LoRa 868MHz) + tcp_0 (Reticulum TCP/HDLC)

## Results

| Metric | Value |
|--------|-------|
| Generations decoded | 246 |
| Generations failed | 0 |
| Symbols received | 500 |
| Decode latency P50 | 0 ms |
| Decode latency P95 | 25 ms |
| Cost incurred | EUR 0,000000 |
| Bearer path | tcp_0 (Reticulum TCP/HDLC) |

## Stress Test (1000 payloads)
| Metric | Value |
|--------|-------|
| Payloads sent | 1,000 |
| Successfully delivered | 830 (83%) |
| Failed (WiFi timeout) | 170 (17%) |
| Bearers per send | 2 |
| Symbols sent | 9,675 |
| API latency P50 | 31.3 ms |
| API latency P95 | 140.9 ms |
| Payload sizes | 64, 140, 237, 340, 454, 500 bytes |

## Bugs Found and Fixed During E2E
1. Stats endpoint read wrong bonder instance
2. Bearer contribution hardcoded to 1
3. Meshtastic PRIVATE_APP not forwarded between nodes
4. SendViaBondGroup wrong bearer priority
5. HeMB frames inside Reticulum headers not detected
6. Stream ID collisions (per-bonder counter)
7. Stream ID wrapping (4-bit → 8-bit)
8. Decoded generations not cleaned from buffer
9. Docker ARM64 GCC QEMU segfault
10. Vue ref not unwrapped in computed property
