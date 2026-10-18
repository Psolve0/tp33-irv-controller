# TP33 IRV Controller

An independent Android app for compatible TP33 RV control panels. Version **1.0.0**.

**[Download v1.0.0 APK](TP33-IRV-Controller-v1.0.0.apk)** · **[Installation instructions](READ-ME-FIRST.txt)** · **[All releases](https://github.com/Psolve0/tp33-irv-controller/releases)**

Open the APK link above, then use GitHub's download button to save the file to your phone. The APK and guides are stored together in this repository. Versioned downloads can also be published under Releases.

**[Download source package](TP33-IRV-Controller-v1.0.0-source.zip)** — includes the source tree, build script and developer documentation.

## Why this app exists

TP33 panels were supplied with instructions for downloading the original **iRV Controller** Android app by Patrick Industries. The AppBrain archive records its last update in April 2017 and removal from Google Play in February 2024. The Android QR shortcut printed on some panels can now lead to a disabled QR-code campaign rather than an app download. These issues can prevent phone control even when the physical panel still works.

This independent replacement was created to restore supported TP33 phone controls on current Android devices, allowing existing hardware to remain useful. It does not require a panel firmware update or hardware replacement. It does not repair a faulty panel. The reason for the original app's removal and its formal support status are not established here; no claim is made about availability of the original iOS app. See [background references](SOURCES.md).

## Requirements and controls

- Android **12 or later** and Bluetooth Low Energy.
- A TP33 panel using the protocol and control mapping described in the [developer guide](DEVELOPER-GUIDE.md).
- Lights, water pump, electric/LP water heater, tank heater and panel power controls.
- Direction buttons for three slide rooms and two awnings.
- Eight-bar battery and tank indicators.
- System light/dark appearance, haptic feedback and local diagnostics.

A similar faceplate or Bluetooth name does not establish compatibility with every command. Read the [panel identification guide](COMPATIBILITY.md) before connecting a different controller.

## Panel identification

![Rockwood-branded TP33 panel reference](panel-reference.png)

Privacy-edited reference image derived from a panel photograph. Reflections and surrounding details were removed; embedded metadata was stripped. Use the image to recognize the face/layout, not to read exact meter levels or scan its pictured QR code. Appearance alone does not establish compatibility.

## Install and connect

1. Open the [APK file](TP33-IRV-Controller-v1.0.0.apk) and use GitHub’s download button to save it.
2. Open it from Downloads/My Files and permit that source to install apps if Android requests it.
3. Open **TP33 IRV**, enable Bluetooth and grant Nearby devices permission.
4. Use **Advanced → Find TP33** to select your panel, or enter its address manually. No address is built into the app.
5. Return to Controls and wait for the connection and panel state to be available.

Menu names vary by phone. [READ-ME-FIRST.txt](READ-ME-FIRST.txt) explains installation, connection, permissions, updates and common problems. Builds 0.1–0.5 use a different signing identity and require a one-time uninstall before installing 1.0.0; later official updates should install over the existing release.

Hold a slide/awning direction for **250 ms** to activate and release to request STOP. Each activation is capped at **five seconds**. Keep physical controls accessible: a lost Bluetooth link or terminated app can prevent STOP delivery. The app does not measure slide/awning position.

## Privacy

The app has no Internet permission, accounts, analytics or automatic uploads. Each selected panel address is stored locally. Diagnostics retain at most the latest minute in memory and clear when the app restarts.

**Copy and Save do not automatically send diagnostics to anyone.** Copy places text on the clipboard; Save writes to the chosen destination. You decide whether to share a copy. Exports may include your panel address and phone information; review them before posting. Clipboard services and chosen storage destinations have their own policies.

## Documentation and contributions

- [Installation and troubleshooting](INSTALLATION.md)
- [Panel identification and compatibility](COMPATIBILITY.md)
- [Architecture, protocol and adaptation guide](DEVELOPER-GUIDE.md)
- [Contributing and other panel variants](CONTRIBUTING.md)
- [Package identity, signing and checksums](PACKAGE.md)
- [Background and platform references](SOURCES.md)

Project home: https://github.com/Psolve0/tp33-irv-controller

For questions or compatibility contributions, use [GitHub Issues](https://github.com/Psolve0/tp33-irv-controller/issues) when available. Never post private signing keys, personal addresses or unredacted diagnostics. The source-license choice remains pending.

## Credits

Developed collaboratively by **Charles Anderson and ChatGPT (OpenAI)**.

- **Primary software development:** ChatGPT.
- **Product direction, co-design and hardware integration:** Charles Anderson.

This is fresh replacement code implementing the TP33 Bluetooth protocol, not a patched or repackaged original APK. No original app code or artwork is bundled. Hardware labels and logos shown in the reference image remain associated with their respective companies. The project is not affiliated with or endorsed by Patrick Industries, iRV Technologies or OpenAI.
