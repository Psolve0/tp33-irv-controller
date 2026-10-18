# Background and implementation references

## Original Android app

TP33 panels were supplied with a printed label advertising the original iRV Controller app by Patrick Industries for Google Play and the App Store. The AppBrain archive records Android package `com.threeobject.rvmobile.le`, version 1.1.2, last updated April 18, 2017 and removed from Google Play February 26, 2024:

https://www.appbrain.com/app/irv-controller/com.threeobject.rvmobile.le

These dates are attributed to the archive, not an official manufacturer discontinuation announcement. The reason for removal and formal support status are not established here. The Android QR shortcut printed on some panels can lead to a disabled-campaign notice from its redirect provider. That does not imply a scanner fault or establish whether an iOS route remains available.

This independent app was created to restore supported phone controls for functioning TP33 hardware on current Android devices. It does not repair a physical panel or claim to be the only possible compatible implementation.

## Protocol reference

Static inspection of the archived original Android APK informed the transport, packet format and UI mapping. Reference APK SHA-256:

`adf4a9c9806417b7a7dc4699e42d00a683b92b2e1cc8491eaa08ac1c0d06d18c`

The archive came from APKPure rather than an independently authenticated manufacturer distribution. The replacement source is new code. Neither the original APK nor decompiled code is bundled, and no original app artwork is reused. The developer guide documents the implemented protocol directly so future adaptations need not depend on a personal development narrative.

## Official platform references

- Android GATT: https://developer.android.com/develop/connectivity/bluetooth/ble/connect-gatt-server
- Android minimum SDK: https://developer.android.com/guide/topics/manifest/uses-sdk-element
- APK distribution/install permission: https://developer.android.com/distribute/marketing-tools/alternative-distribution
- App signing: https://developer.android.com/studio/publish/app-signing
- apksigner: https://developer.android.com/tools/apksigner
- Samsung Auto Blocker: https://www.samsung.com/us/support/answer/ANS10003636/
- Android developer verification: https://developer.android.com/developer-verification
- GitHub Releases: https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases

## Project

- Home and documentation: https://github.com/Psolve0/tp33-irv-controller
- Downloads: https://github.com/Psolve0/tp33-irv-controller/releases
- Questions and contributions: https://github.com/Psolve0/tp33-irv-controller/issues
