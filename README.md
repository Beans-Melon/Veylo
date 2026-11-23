# Veylo

**Your passwords. Your control.**

Veylo is a local password manager for Android, Windows and Chrome. It brings encrypted logins, Important Notes, password generation, migration, recovery tools and deliberate credential use into a minimalist interface.

Created by **ElctroZeus** · **CodeLineCore**

> **Test software:** these are unsigned/private test builds, not an independently audited production password manager. Use dummy records first and keep verified backups.

## Downloads

[Open the download release](https://github.com/Beans-Melon/Veylo/releases/tag/test-2026-11-23) · [Visit the website](https://beans-melon.github.io/Veylo/)

| Package | Version | Requirements |
| --- | --- | --- |
| [Windows installer](https://github.com/Beans-Melon/Veylo/releases/download/test-2026-11-23/Veylo-0.29.0-Windows-Setup.exe) | 0.29.0 | Windows 10/11, x64 |
| [Android APK](https://github.com/Beans-Melon/Veylo/releases/download/test-2026-11-23/Veylo-0.28.0-Local-Test.apk) | 0.28.0 | Android 11+ |
| [Chrome extension ZIP](https://github.com/Beans-Melon/Veylo/releases/download/test-2026-11-23/Veylo-0.28.0-Chrome.zip) | 0.28.0 | Desktop Chrome 133+ |
| [Chrome Wi-Fi sync companion](https://github.com/Beans-Melon/Veylo/releases/download/test-2026-11-23/Veylo-0.27.0-Windows-Companion.zip) | 0.27.0 | Windows x64; compatible with Chrome/Android 0.28.0 |

Different version numbers are intentional. This release collects the latest prepared build for each platform; it does not imply that all platforms have identical features.

**Source archives** for Android/Chrome 0.28.0 and Windows 0.29.0, the Windows guide, and SHA-256 checksums are attached to the release. This repository hosts the product site and documentation; application source is provided in those versioned archives.

## Full feature documentation

Read **[FEATURES.md](FEATURES.md)** for professional explanations of **48 implemented or experimental features**, platform availability, operational limits and a separate development roadmap.

- **Vaults:** AES-256-GCM storage, Argon2id key derivation, independent vaults, passphrase/unlock-word protection and automatic locking.
- **Everyday use:** structured logins, Important Notes, copy/reveal controls, search, groups, favorites and a configurable password generator.
- **Recovery:** ten-version entry history, Recently Deleted, confirmed erasure and encrypted backups.
- **Migration:** reviewed 1PUX/CSV import, duplicate/conflict handling and CSV export.
- **Android:** supported strong biometrics, optional configured NFC YubiKey, native-app autofill, local English/Arabic photo-to-text notes and Bluetooth keyboard Auto-Type.
- **Chrome:** deliberate HTTPS-origin-matched filling, opt-in floating account launcher and per-site permissions.
- **Windows:** standalone installer, built-in local Wi-Fi relay, lifecycle locking and conditional clipboard clearing.
- **Appearance:** custom vault name, JPG/GIF profile image, background color and adaptive text contrast.

## Sync status — read before testing

Local Wi-Fi sync is implemented, with QR-assisted pairing, reviewed merges and saved-record verification. Chrome requires the separate companion; Windows 0.29.0 includes its own relay. Physical phone-to-Windows validation remains necessary.

**Bluetooth data-sync discovery is unresolved.** Bluetooth keyboard Auto-Type is a separate function. Windows 0.29.0 does not expose Bluetooth data sync.

**Veylo Link is in development.** These downloads do not automatically synchronize the Android, Windows and Chrome vaults as one three-way system. Forge Sentinel browser integration is also not included.

## Installation

### Windows

Run the installer and open Veylo. Choose **Create new vault** or **Restore encrypted backup**. Use a standard Veylo `.vvlt` backup and its original credentials. See [WINDOWS-GUIDE.md](WINDOWS-GUIDE.md). Windows may show an unknown-publisher warning because this test build is not Authenticode-signed.

### Android

Back up the existing vault first. The APK updates **Veylo Local Test 0.27.0** in place; it does not replace the older separate **Veylo Multi** app. Do not uninstall an existing app to work around an update failure: uninstalling can delete its local vault data.

### Chrome

Extract the ZIP completely. Open `chrome://extensions`, enable Developer mode, choose **Load unpacked**, and select the extracted `extension` directory containing `manifest.json`. For an existing Veylo installation, replace the contents of its existing extension folder and press **Reload** instead of removing it. Retaining the extension identity and storage matters.

For Chrome local Wi-Fi sync, extract the companion ZIP and follow its included installation instructions. No Python or Google Cloud setup is required.

## Security and feedback

See [SECURITY.md](SECURITY.md). Never include real passwords, vault files, unlock words, QR pairing secrets or signing keys in issues. For ordinary bugs, report the platform/build, exact error, reproduction steps and redacted screenshots. Describe expected and actual behavior.

## About the name

**Veylo** draws on the idea of a veil: a protective layer the user chooses when to lift. The interface aims to make revealing, copying, filling and transferring credentials explicit actions.

No additional open-source license is granted by this repository. Bundled third-party components retain their own licenses in the source and application packages.
