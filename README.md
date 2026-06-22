Payment Confirmation Device Functional Specification V1.3

**Table of Contents**

[1 Preliminary Declarations 4](#_Toc227847015)

[1.1 Disclaimer 4](#_Toc227847016)

[1.2 Foreword 4](#_Toc227847017)

[2 Scope 4](#_Toc227847018)

[3 Terminologies 5](#_Toc227847019)

[4 Generals 6](#_Toc227847020)

[4.1 Design Principles 6](#_Toc227847021)

[5 Operational Ratings 6](#_Toc227847022)

[5.1 Latency 6](#_Toc227847023)

[5.2 Audio Performance 6](#_Toc227847024)

[5.3 Connectivity 6](#_Toc227847025)

[6 Connection and Transport 7](#_Toc227847026)

[7 Device Classifications 7](#_Toc227847027)

[7.1 By Purpose 7](#_Toc227847028)

[7.2 By Connectivity 7](#_Toc227847029)

[7.3 By Power 7](#_Toc227847030)

[8 Marking and Traceability 7](#_Toc227847031)

[9 Functional Requirements 8](#_Toc227847032)

[9.1 Host Model 8](#_Toc227847033)

[9.2 Ordering and Idempotency 8](#_Toc227847034)

[9.3 Audio Confirmation 8](#_Toc227847035)

[9.4 Physical Buttons 8](#_Toc227847036)

[9.5 Fault Recovery 8](#_Toc227847037)

[10 Hardware Requirements 9](#_Toc227847038)

[11 Software Requirements 9](#_Toc227847039)

[11.1 Operating System 9](#_Toc227847040)

[11.2 OTA Updates 9](#_Toc227847041)

[11.3 Protocols and APIs 9](#_Toc227847042)

[11.4 Sound Quality 9](#_Toc227847043)

[12 Security Requirements 10](#_Toc227847044)

[12.1 Secure Boot 10](#_Toc227847045)

[12.2 Cryptographic Profiles 10](#_Toc227847046)

[12.3 Key Custody 10](#_Toc227847047)

[13 Minimum API Support 10](#_Toc227847048)

[14 QUALITY & CONFORMITY OF PRODUCTION (COP) 11](#_Toc227847049)

[14.1 COP Metrics 11](#_Toc227847050)

[14.2 Non-Conformity Classes and Corrective Action (CAPA) 11](#_Toc227847051)

[15 Annex A - Normative Conformance Matrix 12](#_Toc227847052)

[16 ANNEX B - Manufacturing and Quality Checklists (Informative) 14](#_Toc227847053)

[16.1 Bank / Fintech Pre-Order Checklist 14](#_Toc227847054)

[16.2 Pre-Production Checklist (Manufacturer) 14](#_Toc227847055)

[16.3 Sampling Plan and Ongoing Tests 14](#_Toc227847056)

[16.4 Traceability and Serialization 15](#_Toc227847057)

[16.5 PKI and Key Management in Manufacturing 15](#_Toc227847058)

[16.6 OTA Signing and Release Management 15](#_Toc227847059)

[16.7 Field Surveillance and Audit Frequency 15](#_Toc227847060)

[17 ANNEX C (Informative) - Reference Architecture Design 16](#_Toc227847061)

[17.1 Hardware Reference 16](#_Toc227847062)

[17.2 Key interconnects 16](#_Toc227847063)

[17.3 Firmware reference (tasks & modules) 16](#_Toc227847064)

[17.4 Secure Boot & Attestation 17](#_Toc227847065)

[17.5 Provisioning & QR↔Device mapping 17](#_Toc227847066)

[17.6 Mechanical and serviceability 17](#_Toc227847067)

[18 ANNEX D Testing and Certification Process 18](#_Toc227847068)

[18.1 External Standard Complaince 18](#_Toc227847069)

[18.2 Test Cases 18](#_Toc227847070)

# Preliminary Declarations

## Disclaimer

The specifications, documentation, and related materials (collectively, the "Specifications") are provided on an "AS-IS" and "AS-AVAILABLE" basis. No representations or warranties of any kind, express or implied, are made regarding the accuracy, completeness, reliability, or suitability of the Specifications for any particular purpose.

No derivative works or modifications of these Specifications may be created, published, or distributed without the prior written consent of the National Payments Corporation of India. Requests for derivative authorization should be directed to the designated contact address.

## Foreword

This document defines minimum functional, electrical, software, security, and quality requirements for payment confirmation devices - including soundbox devices - that provide real-time, secure payment confirmations at merchant counters. It adopts the clause ordering of the reference standard to facilitate future harmonization with national and international regulatory bodies. The document is intended for review by stakeholders across the payments, banking, and IoT device ecosystems. This specification defines the standardized performance and functional good to have features for payment confirmation devices.

The goal is to ensure a consistent merchant and consumer experience by establishing minimum technical requirements for audio clarity, transaction latency, and device reliability.

# Scope

This specification applies to self-powered and mains-powered merchant payment confirmation devices. These devices are designed to receive encrypted payment notifications from authorized payment rails, decrypt them locally, and play audible confirmations within a defined latency - without exposing PII or any commercially sensitive data to intermediaries.

The scope covers the full device lifecycle: hardware selection and firmware integration, transport and security architecture, marking, testing, and Conformity of Production (COP). It provides guidance to OEMs on hardware, software, and firmware selection and integration.

# Terminologies

| Annotation | Definition                                   |
| ---------- | -------------------------------------------- |
| PSP        | Payment Service Provider                     |
| COP        | Conformity of Production                     |
| CTS / VTS  | Compatibility Test Suite / Vendor Test Suite |
| SPL        | Sound Pressure Level                         |
| OTA        | Over-the-Air Update                          |
| MQTTS      | Message Queuing Telemetry Transport over TLS |
| RTC        | Real-Time Clock                              |
| SE         | Secure Element                               |
| HSM        | Hardware Security Module                     |
| PII        | Personally Identifiable Information          |
| IMEI       | International Mobile Equipment Identity      |
| IMSI       | International Mobile Subscriber Identity     |
| ICCID      | Integrated Circuit Card Identifier           |
| MES        | Manufacturing Execution System               |

# Generals

## Design Principles

Devices shall be designed so that under normal use the following properties hold:

- Secure key management is supported for all payment transmissions.
- Multiple communication protocols may be supported where required by the deployment model.
- Confirmations are played with bounded latency and in correct sequential order.
- Sensitive data and cryptographic keys are never exposed to intermediaries or written to logs.
- Battery and compute resources are selected appropriately for the product variant (screen vs. non-screen).
- Debug access and unused services are disabled in production firmware.

Additional General Metrics: Device quality is evaluated across the following dimensions: performance, reliability, security, interoperability, maintainability, serviceability, traceability, and user experience. These pillars ensure the device is a maintainable and traceable asset within the broader financial infrastructure.

# Operational Ratings

## Latency

The device shall play the audio confirmation within 10 seconds from PSP confirmation. Any delay beyond this threshold creates a checkout bottleneck, erodes merchant confidence, and may introduce time-of-check to time-of-use (TOCTOU) vulnerabilities. Compliance is measured at the 95th percentile under burst conditions.

## Audio Performance

The speaker shall deliver a Sound Pressure Level (SPL) of 60 dB or greater at 3 metres, with clear, intelligible speech in a noisy retail environment. Total Harmonic Distortion (THD) must remain within the limits defined by the Compatibility Test Suite (CTS).

## Connectivity

Devices must maintain a persistent, secure internet connection. Cellular 4G with 2G fallback is the mandatory baseline. Wi-Fi may be used for provisioning or high-density deployments.

# Connection and Transport

- Devices shall support HTTPS and MQTTS over TCP/IP with TLS 1.2 or higher, with full server authentication and certificate-chain validation.
- Clear-text HTTP and MQTT may be supported in laboratory/development environments only; they must not be used in production.
- Transport neutrality may be supported where end-to-end payload security is preserved.
- The device must reject connections with invalid, expired, or mismatched certificates.

# Device Classifications

## By Purpose

- Merchant payment confirmation device - with or without display.

## By Connectivity

- Cellular-only.
- Cellular + Wi-Fi.
- With or without BLE.

## By Power

- Mains-powered with battery backup.
- Battery-dominant (primary power source is a battery).

# Marking and Traceability

Each device shall be marked with its Serial Number and IMEI, both in human-readable form and as a machine-readable barcode, accessible without opening the enclosure. This facilitates field traceability and asset management throughout the device lifecycle.

Each unit must be traceable from component lots to its final Serial Number and IMEI, including the secure-element identity where applicable. Barcodes and digital records shall be reconciled at pack-out; any discrepancy shall block shipment.

# Functional Requirements

## Host Model

The device shall process requests from a single upstream host. This deliberate design choice minimizes the attack surface and simplifies routing and authentication logic at the firmware level, with state managed centrally at the host.

## Ordering and Idempotency

The device shall maintain playback sequence integrity and strictly reject duplicate transaction IDs. First-In, First-Out (FIFO) ordering must be preserved during burst traffic to prevent any single transaction from being played more than once.

## Audio Confirmation

The device shall play localized audio assets for each confirmed transaction. Replay and language-toggle are optional user-interface features. Audio assets must be stored securely on device.

## Physical Buttons

Power, Volume, and Reset buttons are mandatory. Replay and Language-toggle buttons may optionally be provided. All buttons must implement hardware debounce and long-press detection for Reset.

## Fault Recovery

The device shall implement health pings, watchdog monitoring, safe-mode operation, and a self-test routine executed on every boot. These mechanisms ensure the device can recover autonomously from transient failures without manual intervention.

# Hardware Requirements

- **Compute**: Dual core ≥ 500 MHz (or equivalent performance meeting the latency CTS) shall be provided.
- **Memory**: RAM ≥ 16 MB shall be available; App space ≥ 2 MB shall be provisioned.
- **Storage**: Flash ≥ 64 MB shall be available for audio assets, logs, OTA and rollback.
- **Audio path**: 4 Ω / 3 W speaker shall be provisioned to meet SPL (Sound Pressure level).
- **Power**: USB‑C shall be used for power/service; Battery ≥ 2000 mAh shall support outages.
- **Connectivity**: SIM for 4G/2G fallback shall be present; Wi‑Fi/BLE/eSIM optional.
- **Enclosure & Weight**: compact and < 500 g may be targeted (non-conformance-critical).

# Software Requirements

## Operating System

The device shall run Linux or a Real-Time Operating System (RTOS) that supports multi-threaded processing. The OS must be capable of concurrently handling cryptographic operations, network I/O, and audio playback without latency degradation.

## OTA Updates

Firmware updates must be cryptographically signed by the production key and applied atomically. If an update is interrupted - including by a power cut - the device must automatically roll back to the last known-good firmware image. Staging keys are prohibited on production devices.

## Protocols and APIs

HTTPS and MQTTS shall be supported as mandatory transport protocols. HTTP and MQTT may optionally be available for lab use. Logging APIs must scrub all PII and cryptographic material before writing to storage. Error handling must use standardized error codes with documented recovery actions.

## Sound Quality

WAV and MP3 formats shall be supported as a minimum. Audio file bitrate alone is not used as a pass/fail criterion; intelligibility, SPL, and distortion measurements govern validation outcomes.

# Security Requirements

## Secure Boot

The device shall implement a Secure Boot chain rooted in an immutable hardware root of trust. The boot process must cryptographically verify the bootloader, kernel, and application layer using signed images. Any tampered image must be refused, preventing execution of unauthorized firmware.

## Cryptographic Profiles

- Content encryption: AES-256.
- Identity and signing: RSA-2048+ or Elliptic Curve Cryptography (ECC). ECC is recommended for its resistance to side-channel attacks and superior performance in resource-constrained environments.
- Digest: SHA-512.

## Key Custody

Cryptographic keys shall be generated and stored within the Secure Element (SE) or HSM. Keys must never be hard coded in firmware, written to the filesystem in plaintext, or exposed in device logs. Production keys must be generated under HSM with dual-control authorization, and all provisioning operations must be fully auditable.

# Minimum API Support

- **Audio**: play WAV/MP3, queue control, volume shall be provided.
- **Files**: CRUD, size and free space shall be provided; unzip optional.
- **HTTP(S)**: GET/POST with headers; MQTTS: subscribe/publish with X.509 TLS shall be provided.
- **Device Info**: serial, model, firmware version shall be query able.
- **SIM/NTP/Power**: IMSI/IMEI/ICCID + signal; NTP sync + RTC; charge % & health shall be provided.
- Note: Most Soundboxes use a hybrid approach: The RTC (Real time clock) keeps the time locally to save power, and the device performs an NTP (Network Time Protocol) sync only when it's already "awake" to process a payment, or once every 24 hours to correct any drift.
- **LED and Timer APIs optional.**

# QUALITY & CONFORMITY OF PRODUCTION (COP)

## COP Metrics

Device quality is tracked through the following measurable indicators: transaction latency, success rate, throughput, uptime, error rate, recovery time, and battery endurance. The device must support export of a health snapshot as COP evidence for audit purposes.

## Non-Conformity Classes and Corrective Action (CAPA)

| Class    | Definition                                                                 | Action                                              | Closure SLA                              |
| -------- | -------------------------------------------------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| Critical | Security/key exposure; incorrect amount announcement; wrong-order playback | Immediate stop-ship; recall if in-field; hotfix OTA | 7 days (Containment); 30 days (Closure)  |
| Major    | Latency or SPL outside spec; OTA rollback failure                          | Stop-ship; corrective rework; field patch as needed | 14 days (Containment); 45 days (Closure) |
| Minor    | LED/UX inconsistency; cosmetic enclosure issue                             | Rework in next lot; no stop-ship                    | Next production lot                      |

For Critical and OTA Rollback items, the Acceptable Quality Level (AQL) is zero. A single failure in either category indicates a fundamental compromise of root-of-trust or fleet stability, rendering the entire production lot untrustworthy and triggering immediate recall or stop-ship procedures.

# Annex A - Normative Conformance Matrix

| Requirement                                                | Verification Method                                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Process HTTPS/MQTTS over TCP/IP with TLS1.2+               | Connect to CTS broker/server; validate chain & hostname; MITM negative test passes |
| Play confirmation ≤ 10 s from client confirmation          | Burst 10 txns; 95p ≤ 10 s; no re-ordering                                          |
| Maintain sequence & reject duplicates                      | Send out-of-order and duplicate txn IDs; only one playback per ID in FIFO order    |
| Buttons:<br><br>Power/Volume/Reset.<br><br>Replay/Language | Physical actuation tests; debounce; long-press for Reset                           |
| Dual core ≥ 500 MHz (or equivalent)                        | Latency CTS passes under crypto+audio load                                         |
| RAM ≥ 16 MB, App ≥ 2<br><br>MB, Flash ≥ 64 MB              | Memory map review; soak test with 10-txn bursts; OTA/rollback space verified       |
| Speaker 4 Ω / 3 W to meet SPL                              | Acoustic @3 m meets CTS SPL; harmonic distortion within limit                      |
| USB-C power/service                                        | Charge, service mode entry via USB-C                                               |
| 4G with 2G fallback; Wi‑Fi recommended                     | Cellular attach across bands/APNs; Wi‑Fi throughput (if present)                   |
| Barcode Serial & IMEI                                      | Scan under 300-500 lx; decode correctly                                            |
| OS (Linux/RTOS); logging tools & docs                      | Build/flashing reproducible; logs scrub PII/keys                                   |
| OTA signed, atomic, rollback                               | Signature validation: power-cut during apply; rollback success                     |
| Secure boot, signed binaries                               | Boot chain attestation; tampered image refused                                     |
| AES‑256, RSA‑2048+ /<br><br>ECC, SHA‑512                   | Crypto self-tests; identity provisioning; ECC acceptance if implemented            |
| Audio WAV/MP3 + queue control                              | Playlists; pre-emption rules; volume curve                                         |
| HTTP(S) + MQTTS APIs                                       | Positive/negative API suite                                                        |
| Device info, SIM,<br><br>NTP/RTC, Power                    | Fields populated; RTC drift within CTS bounds                                      |
| Quality metrics captured<br><br>& reported                 | Export health snapshot; COP audit<br><br>trail                                     |
| COP with audits                                            | Factory sampling and field spot-checks                                             |
| Unzip, LED API,<br><br>Memory/Timer APIs                   | If present, pass functional tests                                                  |
| Weight < 500 g,<br><br>"compact/portable"                  | Informative - not a cause for failure                                              |
| Sound quality > 128 kbps                                   | Informative - SPL/clarity govern pass/fail                                         |

# ANNEX B - Manufacturing and Quality Checklists (Informative)

This annex provides practical checklists and rationale for manufacturers and procurement teams. Clause ordering mirrors the referenced Indian Standard to ease future harmonization with formal certification bodies.

## Bank / Fintech Pre-Order Checklist

Prior to accepting delivery, customers may request the following artefacts: COP Plan, Control Plan, Traceability Matrix, PKI SOP, OTA Signing SOP, Audit Reports, and CAPA Register. These artefacts establish compliance readiness and production control evidence.

## Pre-Production Checklist (Manufacturer)

Before production commences, manufacturers must freeze the approved hardware revision, firmware image hash, crypto profile, and CTS/VTS reports. Benchmark units, test vectors, boot-chain keys, and production signing keys must be fixed and locked in the HSM under dual control before release.

## Sampling Plan and Ongoing Tests

| Check                   | Method                            | Units/Lot | Acceptance (AQL) | Notes                                 |
| ----------------------- | --------------------------------- | --------- | ---------------- | ------------------------------------- |
| Secure Boot & Signature | Cold boot with tampered image     | 5         | 0 failures       | Boot chain must refuse tampered image |
| Latency @ Burst         | Inject 10 encrypted notifications | 8         | 95p ≤ 10 s       | No reorder; no duplicate playback     |
| SPL @ 1 m               | Anechoic or controlled ambient    | 8         | Meets CTS SPL    | Volume curve within spec              |
| Cellular Attach         | Multi-APN attach/retry            | 8         | 100% attach      | Log signal metrics                    |
| OTA Rollback            | Power cut during apply            | 3         | 0 failures       | Rollback to prior image               |
| Key Presence            | SE/HSM key attestation            | Sampling  | 100% present     | No keys in filesystem                 |
| Traceability            | Scan S/N & IMEI barcode           | All       | 100% readable    | Matches MES records                   |

## Traceability and Serialization

- Each unit must be traceable from component lots to its final Serial Number and IMEI, including SE identity.
- Barcodes and digital MES records must be reconciled at pack-out; discrepancies block shipment.

## PKI and Key Management in Manufacturing

- Private keys must be generated inside the SE or HSM and must never be exportable.
- Certificate signing requests (CSR) shall be signed by production CAs under HSM, with dual control.
- All cryptographic provisioning operations must be fully auditable.

## OTA Signing and Release Management

- Firmware and data bundles must be signed with production keys; staging keys are prohibited on production devices.
- Each release must define rollback targets and migration scripts; delta update strategies are permitted where rollback remains deterministic.
- Release notes must include known issues, CVEs addressed, and updated firmware hashes.

## Field Surveillance and Audit Frequency

- Post-shipment: 1% field audit per quarter across geographies.
- Telemetry review covering latency, uptime, and error codes; anomalies must trigger formal CAPA.

# ANNEX C (Informative) - Reference Architecture Design

This annex provides a vendor-agnostic reference design for hardware and firmware meeting the requirements of Sections 9-13. Implementers may substitute equivalent components provided conformance is demonstrated through CTS.

## Hardware Reference

- Application / Modem SoC: LTE Cat-4 module (e.g., Quectel EC200U) providing CPU, cellular stack, PCM/I2S audio, UART, SDIO, and GPIO.
- Wi-Fi / BLE Companion: 2.4 GHz MCU (e.g., ESP32-C3) for Wi-Fi provisioning and optional BLE.
- Audio Path: Class-D amplifier driving a 4 Ω / 3 W speaker.
- Storage: ≥ 64 MB external flash for audio assets, logs, OTA, and rollback.
- Power: 5 V USB-C input; buck regulators to 3.8 V and 3.3 V; battery pack ≥ 2000 mAh with fuel gauge and charge management IC.
- I/O and UI: Power, Volume, and Reset buttons; two status LEDs; optional small display.

## Key interconnects

| Interface | From → To           | Purpose                                                         |
| --------- | ------------------- | --------------------------------------------------------------- |
| UART      | SoC ↔ Wi‑Fi MCU     | Wi‑Fi control / provisioning; AT host or custom framed protocol |
| PCM/I2S   | SoC → Amp → Speaker | Audio playback for confirmations                                |
| SDIO/SPI  | SoC ↔ Flash         | Assets, logs, OTA bundles                                       |
| I²C       | SoC ↔ Fuel Gauge/SE | Battery metrics; secure element access                          |
| USB‑C     | VBUS/CC → PMIC      | Power input, service mode                                       |

## Firmware reference (tasks & modules)

- **ConnectivityManager:**
  - GSM/Wi‑Fi attach, APN selection, backoff, health pings.
- **MQTTProcessor:**
  - persistent MQTTS session; payload validation; handoff to SDK; audio events.
- **UPCS‑SDK:**
  - ECDH key exchange; AES‑GCM decrypt; HMAC/RSA verification; key rotation.
- **AuthenticationManager:**
  - Device login/token lifecycle; provisioning handshakes.
- **AudioPlayback:**
  - Localized WAV/MP3 assets; queueing; replay; volume curve; multilingual.
- **UpdateManager:**
  - Signed OTA, atomic apply, rollback on failure; diff updates recommended.
- **DeviceSetup/Buttons/BatteryManager:**
  - Provisioning, input handling, fuel gauge.
- **Logging:**
  - Scrubbed structured logs; PII/keys excluded; rate limited.

## Secure Boot & Attestation

- Secure Boot: immutable root verifies bootloader → kernel → app using signed images.
- Key Generation: performed inside SE/HSM; identity keys non-exportable.
- Device Attestation: challenge/response proving key custody and firmware hash to backend.

## Provisioning & QR↔Device mapping

- Device registration with operator (orgId/appId), then merchant-app binding of multiple QR/terminal IDs to a single device.
- Mapping updates are versioned and auditable; device receives only encrypted minimal payloads (amount + tags).

## Mechanical and serviceability

- Enclosure allows barcode visibility without opening; USB‑C accessible for service.
- Tamper‑evident seals on screws; internal debug ports disabled in production images.

# ANNEX D Testing and Certification Process

## External Standard Complaince

Final product certification requires demonstrated compliance with the following standards:

- Electrical Safety: IS 13252 (Part 1):2010, tested at a BIS-recognised laboratory.
- EMC and Wireless: [3GPP Radio Access](https://ctiacertification.org/wp-content/uploads/2021/02/CTIA-01.50-Wireless-Technology-3GPP-Radio-Access-Technologies-V6_0_5.pdf) [Technologies](https://ctiacertification.org/wp-content/uploads/2021/02/CTIA-01.50-Wireless-Technology-3GPP-Radio-Access-Technologies-V6_0_5.pdf) standards; [CTIA 01.50 Wireless Technology,](https://ctiacertification.org/wp-content/uploads/2021/02/CTIA-01.50-Wireless-Technology-3GPP-Radio-Access-Technologies-V6_0_5.pdf) for cellular OTA testing.
- Cybersecurity: [CTIA](https://ctiacertification.org/wp-content/uploads/2022/02/CTIA-Certification-Cybersecurity-Test-Plan-V2.1.3.pdf) [Cybersecurity](https://ctiacertification.org/wp-content/uploads/2020/10/CTIA-Cybersecurity-Test-Plan-1.2.2.pdf) [Test Plan v2.1.3](https://ctiacertification.org/wp-content/uploads/2022/02/CTIA-Certification-Cybersecurity-Test-Plan-V2.1.3.pdf)
- Bluetooth (if implemented): [Bluetooth SIG](https://www.bluetooth.com/develop-with-bluetooth/qualify/) qualification.
- Thermal: Testing for overheating under sustained operational load.

## Test Cases

| Test Case ID | Title                         | Requirement Reference              | Preconditions                                              | Test Steps                                                                                             | Expected Result                                                 |
| ------------ | ----------------------------- | ---------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| TC-01        | HTTPS/MQTTS TLS Validation    | TLS1.2+                            | Device provisioned with valid certs; CTS broker accessible | 1\. Power ON device 2. Initiate HTTPS 3. Initiate MQTTS 4. Capture TLS logs 5. Verify version & cipher | TLS ≥1.2 negotiated; Valid cipher suite; No handshake errors    |
| TC-02        | Certificate Chain & MITM Test | Chain & Hostname Validation        | Valid broker + MITM proxy setup                            | 1\. Connect to valid broker 2. Attempt MITM cert 3. Attempt hostname mismatch                          | Valid cert succeeds; MITM rejected; Hostname mismatch rejected  |
| TC-03        | Playback Confirmation Latency | ≤10s Confirmation                  | Device connected to network                                | 1\. Trigger txn 2. Start timer 3. Wait playback 4. Record time                                         | Playback ≤10 seconds                                            |
| TC-04        | Burst 10 Transactions         | 95p ≤10s No Reorder                | Network stable                                             | 1\. Send 10 txns rapidly 2. Record timestamps 3. Compare order 4. Calculate 95p latency                | 95p ≤10s; No reordering                                         |
| TC-05        | Sequence & Duplicate Handling | FIFO + Reject Duplicates           | Device online                                              | 1\. Send ordered txns 2. Send duplicate ID 3. Send out-of-order IDs                                    | Only one playback per ID; FIFO maintained                       |
| TC-06        | Button Functional & Debounce  | Power/Volume/Reset/Replay/Language | Device powered                                             | 1\. Short press each 2. Rapid press 3. Long press reset 4. Verify replay/lang                          | No bounce errors; Reset on long press; Replay/lang works        |
| TC-07        | CPU Load Performance          | Dual Core ≥500MHz                  | Performance build enabled                                  | 1\. Run TLS handshake 2. Trigger audio 3. Send burst txns 4. Monitor CPU                               | Latency within CTS; No glitches; No watchdog reset              |
| TC-08        | Memory Validation             | RAM ≥16MB Flash ≥64MB              | Memory map available                                       | 1\. Review memory map 2. Soak test 3. Verify OTA partitions                                            | No overflow; OTA + rollback present; Stable operation           |
| TC-09        | Acoustic SPL Test             | 4Ω/3W Speaker CTS SPL              | Audio test file available                                  | 1\. Play test tone 2. Measure SPL @3m 3. Measure THD                                                   | SPL within spec; THD within limit                               |
| TC-10        | USB-C Power & Service         | USB-C Support                      | USB cable & PC available                                   | 1\. Connect USB-C 2. Verify charge 3. Enter service mode                                               | Charging works; Service mode accessible                         |
| TC-11        | Cellular & Wi-Fi Validation   | 4G + 2G Fallback                   | Valid SIM inserted                                         | 1\. Attach LTE 2. Force 2G fallback 3. Validate APN 4. Test WiFi throughput                            | Network attach success; Fallback works; WiFi stable             |
| TC-12        | Barcode Serial & IMEI         | 300-500lx Decode                   | Barcode under proper lighting                              | 1\. Scan serial 2. Scan IMEI 3. Verify decode                                                          | Correct decoding without errors                                 |
| TC-13        | OS Build & Logging            | Reproducible Build                 | Source repo clean                                          | 1\. Clean build 2. Flash 3. Compare hash 4. Inspect logs                                               | Reproducible build; No PII/keys in logs                         |
| TC-14        | OTA Atomic & Rollback         | Signed OTA + Rollback              | OTA server ready                                           | 1\. Trigger OTA 2. Cut power mid-update 3. Reboot 4. Verify rollback                                   | Signature validated; Rollback successful; Device functional     |
| TC-15        | Secure Boot                   | Signed Binaries                    | Secure boot enabled                                        | 1\. Boot valid image 2. Boot tampered image                                                            | Valid boots; Tampered rejected                                  |
| TC-16        | Crypto Algorithm Self-Test    | AES256 RSA2048 ECC SHA512          | Crypto test suite enabled                                  | 1\. Run self-tests 2. Verify key sizes 3. Sign/verify test                                             | All algorithms pass; Correct key sizes                          |
| TC-17        | Audio Format & Queue          | WAV/MP3 + Queue                    | Audio files available                                      | 1\. Play WAV 2. Play MP3 3. Queue multiple 4. Test preemption 5. Adjust volume                         | Formats supported; FIFO; Preemption works; Volume curve correct |
| TC-18        | API Validation Suite          | HTTPS + MQTTS APIs                 | API test harness available                                 | 1\. Run positive suite 2. Send invalid payload 3. Invalid auth 4. Timeout test                         | Valid succeed; Invalid rejected                                 |
| TC-19        | Device Info & RTC             | NTP/RTC Compliance                 | NTP server reachable                                       | 1\. Query device info 2. Sync NTP 3. Measure 24h drift                                                 | Fields populated; RTC drift within bounds                       |
| TC-20        | Health Snapshot & Audit       | COP Audit Trail                    | Audit logging enabled                                      | 1\. Export health snapshot 2. Verify metrics 3. Review audit entries                                   | Snapshot complete; Audit intact                                 |
| TC-21        | Optional APIs                 | Unzip/LED/Memory/Timer             | APIs implemented                                           | 1\. Test unzip 2. LED control 3. Memory alloc/free 4. Timer accuracy                                   | APIs function correctly                                         |
| TC-22        | Weight Verification           | <500g Informative                  | Calibrated scale available                                 | 1\. Weigh device 2. Record measurement                                                                 | Weight <500g (informative)                                      |
| TC-23        | Sound Quality                 | \>128kbps Informative              | 128kbps audio file available                               | 1\. Play 128kbps 2. Assess clarity 3. Measure SPL/THD                                                  | Clarity acceptable; SPL governs pass/fail                       |

All test cases must be executed on a production-representative build with production-signed firmware. Informative tests (TC-22, TC-23) do not constitute pass/fail criteria but must be documented in the COP audit trail.
