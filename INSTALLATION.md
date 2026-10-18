# Installation and troubleshooting

Read READ-ME-FIRST.txt first. This guide uses ordinary Android names; Samsung and other manufacturers may arrange settings differently. See the official Android and Samsung references below if your screens differ.

## Install a downloaded APK

Download the APK from the main file list at https://github.com/Psolve0/tp33-irv-controller (open its APK entry and use GitHub's download button), or from a published release, open it from Downloads/My Files, and select Install. If prompted, grant **Install unknown apps / Allow from this source** to the browser or file manager that opened the APK. Return to finish installation. You can revoke that source permission afterward. No separate signing key or certificate import is needed; Android verifies the certificate embedded in the signed APK.

Some Samsung devices use Auto Blocker, which can block APK installation. Review its explanation in Settings and Samsung's official guidance. Do not disable unrelated protections or Play Protect as a routine step. Organizational policies or newer Android developer-verification requirements may also restrict installation; the procedure is not a promise that all devices allow sideloading.

## Transition from earlier builds

Version 1.0.0 uses a new signing key and the same application package. Uninstall versions 0.1–0.5 before installing 1.0.0. This one-time transition resets local preferences and permissions. Later releases signed with this release key should update over the installed release when their version code increases.

## First connection

There is no built-in panel address. Tap Connect to open Advanced when no address is saved. Use Find TP33, grant Nearby devices/Bluetooth permission, wait for nearby devices, and select your own panel. Selection starts connecting and saves the selected address. Or enter your panel's address, Save address, return to Controls and Connect. Multiple devices require identifying your own panel; do not connect to a neighbor's equipment. The search looks for advertised names containing TP33; unnamed/differently named variants may need manual entry and further compatibility investigation.

## Common problems

| Symptom | Check |
|---|---|
| App not installed / package conflict | Earlier build still installed? New key cannot replace it without the planned clean install. Also check Android 12 minimum, exact error, file integrity and official source. |
| Permission to install unavailable | Check the source app, Samsung Auto Blocker, device-management policy or Android's current installation restrictions. |
| No TP33 found | Confirm panel power and phone Bluetooth, move closer, disconnect other control apps, and search again. A variant may advertise a different name. |
| Pairing prompt appears | Android/the panel can request pairing. The app does not explicitly remove existing bonds or force re-pairing. |
| Paired in Settings but no Connected label | A custom app-managed BLE/GATT connection may not be represented like a headset/audio profile. Check the app status and fresh RX responses instead. Exact Settings behavior varies by manufacturer. |
| Amber switch badge | Confirmed state is missing. Tap to request state, or use Refresh readings; no ON/OFF guess is sent. |
| Unexpected light/utility response | Stop using that control, record app/panel variant and a sanitized log; do not assume a faceplate match proves mapping. |
| Movement does not stop | Use the physical panel immediately. App STOP depends on the radio link and is not a separate hardware emergency stop. |
| Log has little or no history | Entries expire after 60 seconds and memory clears when the app restarts. A log copied late cannot recover an older incident. Save promptly before restarting. |
| Stale/incorrect meter reading | Refresh readings and compare with the panel. Eight-bar displays reproduce its reports; they cannot fix dirty sensors or calibrate charge. |

## Reporting a problem

Capture the exact error and app version, phone model, Android version, panel model/face variant, intended action and actual behavior. Copy/save diagnostics promptly if useful; review them for personal addresses/device details before sharing. Do not post private signing material or unredacted logs. Project questions: https://github.com/Psolve0/tp33-irv-controller/issues

Downloads: https://github.com/Psolve0/tp33-irv-controller/releases

## Official installation references

- Android alternative distribution: https://developer.android.com/distribute/marketing-tools/alternative-distribution
- Android minimum SDK: https://developer.android.com/guide/topics/manifest/uses-sdk-element
- Samsung Auto Blocker: https://www.samsung.com/us/support/answer/ANS10003636/
- Android developer verification: https://developer.android.com/developer-verification
- App signing: https://developer.android.com/studio/publish/app-signing

Consult the current manufacturer guidance if your screens differ.
