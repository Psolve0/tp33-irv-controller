# Package identity and updates

| Field | Value |
|---|---|
| App version | 1.0.0 |
| Version code | 10000 |
| Application ID | `local.tp33.trial` |
| Launcher label | TP33 IRV |
| Minimum Android API | 31 / Android 12 |
| Target/compile Android API | 35 |
| Required hardware | Bluetooth Low Energy |
| Permissions | BLUETOOTH_SCAN (neverForLocation), BLUETOOTH_CONNECT |
| Internet permission | None |
| Native CPU-specific libraries | None |
| Signing certificate subject | CN=TP33 IRV Controller |
| Certificate SHA-256 | `a1b67743e387ce9d2650f2534b1f024ec43ee74f7f42e6238f36d622e9819208` |

The application ID is a technical identifier; its historical wording does not change the displayed name. The public certificate included in the source can identify the release signer but cannot sign an APK. Private signing material must remain outside the project.

[SHA256SUMS.txt](SHA256SUMS.txt) contains the APK hash. Obtain the APK and hash from the project Releases page. Android verifies the embedded signature when installing or updating an APK.

## Updating

Builds 0.1–0.5 use a different signing identity. Uninstall those builds once before installing 1.0.0; local preferences and permissions reset. Subsequent official releases need a larger version code and the same release signing identity to update the installed app normally.

The v1.0.0 Check for updates button is disabled. Adding a GitHub project URL to documentation does not enable the button or add networking to the APK. Until a later app update implements that feature, download updates manually from:

https://github.com/Psolve0/tp33-irv-controller/releases
