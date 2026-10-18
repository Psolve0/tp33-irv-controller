# Developer guide: TP33 implementation and adaptation

This is fresh Java/Android replacement code, not a patch of the original APK. It implements the TP33 protocol and mapping documented below. Similar devices can map the same IDs to different outputs; adapting one requires establishing its actual command and state behavior.

## Contributions and provenance

Developed collaboratively by Charles Anderson and ChatGPT (OpenAI). Primary software development: ChatGPT. Product direction, co-design and hardware integration: Charles Anderson. No manufacturer or OpenAI endorsement is implied.

The archived iRV Controller 1.1.2 APK served as a reference for transport, packet and UI mappings. No decompiled original app code or artwork is bundled. See SOURCES.md for the archive reference and background.

## Build from source

Requirements:

- Java 17 runtime and keytool.
- Eclipse ECJ 3.37.0 compiler JAR.
- Android SDK Platform 35 (`android.jar`) and Build Tools 35.0.0 (`aapt2`, `d8`, `zipalign`, `apksigner`).
- Python 3 for ZIP packaging.

Set `ANDROID_JAR`, `BUILD_TOOLS` and `ECJ_JAR` to those installed paths. Build Tools binaries must be executable. The source uses Java 8 language output and min API 31. It contains no Gradle dependency graph or external app runtime libraries. `build.sh` compiles Android source, packages the manifest/vector launcher, generates DEX, aligns and signs the APK.

For a personal modified build, create **your own** signing key outside the source tree. Set `RELEASE_KEYSTORE` and `RELEASE_KEY_ALIAS`, and securely supply `TP33_KEYSTORE_PASSWORD` and `TP33_KEY_PASSWORD` in the build process environment, then run `bash build.sh`. Avoid placing secrets in source, command history or logs. The official release private key is not supplied in public source. Building with another key does not create an installable update over the official release; use an independently named application ID for a fork or plan a clean installation. The public certificate included here is for verification, not signing.

Official tools:

- https://developer.android.com/tools
- https://developer.android.com/tools/apksigner
- https://developer.android.com/studio/publish/app-signing
- https://download.eclipse.org/eclipse/downloads/ (ECJ compiler)

This documents a repeatable build procedure, not a guarantee of byte-identical APK output across machines/signing configurations.

## Source map

| File | Responsibility |
|---|---|
| `MainActivity.java` | Three native pages; BLE lifecycle, permissions, discovery, serialized writes, received-state mapping, touch handling and exports |
| `Protocol.java` | Five-byte command construction; buffered notification reassembly/checksum validation and motor IDs |
| `HoldGate.java` | 250 ms deliberate activation gate |
| `SwitchState.java` | Confirmed vs requested toggle state, pending request revisions |
| `RecentLog.java` | Rolling 60-second RAM-only history with entry/character caps |
| `MeterLevels.java` | Eight-bar scale and battery color bands |
| `EightBarView.java` | Meter drawing, per-level uniform battery colors and theme-aware tank colors |
| `FunctionIcon.java` | Native vector-like function/movement paths and state badges |
| `CenteredIconButton.java` | Centers Stop/Advanced icon and label together with inner margins |
| `AndroidManifest.xml` | Package, min/target SDK, permissions and version |
| `res/drawable/ic_launcher.xml` | New trailer launcher artwork |

The package/application ID is `local.tp33.trial`, retained from the earlier builds; the visible name/version is TP33 IRV / 1.0.0. Version code 10000 represents this release; future official updates need a larger integer and the same signing identity.

## BLE transport

| Role | UUID |
|---|---|
| Observed service | `00001910-0000-1000-8000-00805f9b34fb` |
| Receive characteristic, NOTIFY | `0000fff2-0000-1000-8000-00805f9b34fb` |
| Transmit characteristic, WRITE | `0000fff4-0000-1000-8000-00805f9b34fb` |
| Notification CCCD | `00002902-0000-1000-8000-00805f9b34fb` |

Subscribe with CCCD bytes `01 00`. Writes use Write Request/default write type and are serialized, advancing on write callback. The implementation discovers the expected characteristic UUIDs across services rather than treating the service alone as compatibility proof. No alternative UUID pair from the old app has been validated or enabled.

There is **no embedded device address**. Find TP33 discovers advertised names containing TP33, lists nearby devices and connects to the user's selection. Manual entry remains available. The selected address is saved locally. A missing name/address should not be worked around by hard-coding another person's device.

Android pairing/bonding saves security credentials; GATT connection is a separate operation. The app calls `connectGatt`, does not call `createBond`/remove-bond, and lets Android handle necessary security prompts. System Settings may represent only recognized profiles and need not mirror this custom link's app status.

## Outgoing commands

Five binary bytes:

`80 COMMAND STATE 00 CHECKSUM`

`STATE` is 00/OFF or 01/ON. Checksum is the sum of the first four bytes modulo 256. Send binary bytes, not a UTF-8 representation. Do not append incoming suffix bytes to a command.

| Function | Decimal ID | Hex ID |
|---|---:|---|
| Awning light | 1 | 01 |
| Aisle light | 2 | 02 |
| Step light | 3 | 03 |
| Electric water heater | 4 | 04 |
| Slide 2 out / in | 5 / 6 | 05 / 06 |
| LP gas water heater | 7 | 07 |
| Panel power | 8 | 08 |
| Slide 3 out / in | 9 / 10 | 09 / 0A |
| Water pump | 11 | 0B |
| Entry light | 12 | 0C |
| Awning 1 extend / retract | 13 / 14 | 0D / 0E |
| Tank heater | 15 | 0F |
| Porch light | 16 | 10 |
| Slide 1 out / in | 17 / 19 | 11 / 13 |
| Awning 2 extend / retract | 18 / 20 | 12 / 14 |
| Meter request | 27 | 1B |
| Switch/motor snapshot request | 29 | 1D |

Examples: awning light ON `80 01 01 00 82`, OFF `80 01 00 00 81`. Switch request `80 1D 01 00 9E`; meter request `80 1B 01 00 9C`. These IDs are the mappings implemented by this app. Old internal variable names are not reliable labels for another panel; establish the actual function of each ID before adapting the table.

## Incoming framing

Notifications may split a frame or concatenate several frames. Do not parse each BLE notification as a complete message. `Protocol.Parser` buffers fragments, searches for 80, recognizes IDs/lengths, validates checksums and resynchronizes after unknown IDs or invalid checksums.

Zero-based byte positions:

| Type | Parser frame length | Checksum index | Payload interpretation |
|---|---:|---:|---|
| IDs 1–20 | 6 | 4 | byte 2 = reported state; byte 5 is not part of checksum |
| Meter ID 1B | 12 | 10 | fields below |
| Snapshot ID 1D | 7 | 5 | bytes 2–4 little-endian bitfield |

Individual state events often arrive with an extra trailing 00 (for example seven received bytes); the six-byte event parser discards that unframed zero while finding the next header. The full meaning of event suffix bytes is not established. Some additional original-protocol frames remain undecoded; do not invent semantics for them.

### Snapshot bitfield

| Bit | Command/output |
|---:|---|
| 0 | 4 electric heater |
| 1 | 1 awning light |
| 2 | 15 tank heater |
| 3 | 3 step light |
| 4 | 11 water pump |
| 5 | 7 LP heater |
| 6 | 12 entry light |
| 7 | 16 porch light |
| 8 | 2 aisle light |
| 9 / 10 | 17 / 19 slide 1 out / in |
| 11 / 12 | 5 / 6 slide 2 out / in |
| 13 / 14 | 9 / 10 slide 3 out / in |
| 16 / 17 | 13 / 14 awning 1 extend / retract |
| 18 / 19 | 18 / 20 awning 2 extend / retract |
| 23 | 8 panel power |

### Meter fields

ID 1B: byte 2 fresh, 3 black 1, 4 black 2, 5 gray 1, 6 gray 2, 7 galley, 9 battery. The original app's meter XML sets full scale to 8 for all meters. The replacement shows 0–8 completed levels, clamps larger values and distinguishes unavailable (-1) from empty (0). Individual sensor accuracy is not verified; battery bars are relative panel codes, not calibrated state of charge. Battery color is selected from the displayed bar count (1–2 red, 3–5 yellow, 6–8 green) and applied uniformly to every filled segment.

## State, movement and lifecycle

A toggle tracks the panel's confirmed state separately from a pending target. Unknown-state taps request a snapshot rather than guessing ON/OFF. A confirmation timeout marks state unavailable and refreshes; it does not automatically replay a control command.

Movement starts only after the 250 ms gate. Release/cancel, leaving the button, scroll intent during a pending hold, multitouch, navigation, focus loss and backgrounding cancel activation or enqueue STOP. Starts and stops use the existing serialized transport, with stops prioritized. Each activation has a five-second cap. Stale queued ON requests are dropped; no automatic reconnect/motion replay is added. Theme changes update existing views and cancel/stop an outstanding hold.

A GATT write success proves transport acceptance, not physical movement or stopping. Link loss, process termination and phone failure can prevent STOP delivery. These mechanisms do not replace physical controls or a hardware emergency stop. Direction reports do not establish measured position, endpoint or percentage of travel. Do not add a position estimate without separate evidence.

## Diagnostics

`RecentLog` uses monotonic time and retains only the last 60 seconds, capped additionally at 1,200 entries / 180,000 characters. History is RAM-only, does not grow a disk file and clears on process/activity recreation. A normal link disconnect does not deliberately clear history. Background-delivery bursts are timestamped when the app processes them; wall timestamps are not guaranteed radio arrival or sensor times.

Copy/Save returns the retained window. Snapshot exports current app state and latest RX/TX/readings with timestamps/age, not historical event lines or a fresh simultaneous measurement. Exported data may contain the user's panel address, phone model and OS; the user must review/redact before sharing. Export actions do not send data automatically. A minimal log after an incident cannot establish its cause once history has expired or the app restarted.

## Adapting another panel

1. Identify its actual panel variant and observe advertised services/characteristics and notifications using authorized equipment. Similar branding/UUIDs are not sufficient proof.
2. Start with passive capture. Operate one physical control at a time and correlate its notifications, framing and state changes. Deduplicate a capture tool's repeated display/log lines.
3. Keep unverified controls disabled in the fork. Review UUIDs and name filter in MainActivity, packet framing/IDs in Protocol, UI mappings and snapshot bit positions in MainActivity, meter fields/scales, and FunctionIcon labels/paths.
4. Confirm a low-impact output and its OFF behavior while directly observing the equipment. A matching checksum or echoed command is not proof of correct mapping.
5. Validate status synchronization, split/combined frames, pending request handling, disconnects, cancellation and STOP behavior. Only then enable movement under direct supervision with clearances and physical controls available.
6. Keep variant-specific protocol and integration notes. Never silently generalize this TP33's IDs or full-scale meter value.
7. Preserve parser/state/timing safeguards while adapting the implementation. Keep private addresses, original personal logs, signing keys and passwords out of the repository.

Project home: https://github.com/Psolve0/tp33-irv-controller

Questions and proposed adaptations: https://github.com/Psolve0/tp33-irv-controller/issues

A source license has not yet been selected. Keep private signing material outside the repository and never distribute it with a fork.
