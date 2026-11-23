# Veylo for Windows — Test Build 0.29.0

Standalone Windows 10/11 x64 desktop application, based on Veylo 0.28.0. Created by ElctroZeus. No Python, Google account, Google Cloud setup, or separately installed sync helper is required.

## Install and open

1. Run `Veylo-0.29.0-Windows-Setup.exe`. Choose your installation folder and finish setup.
2. Open **Veylo** from the Start menu or desktop shortcut.
3. Choose **Create new vault** or **Restore encrypted backup**.

This test installer is not Authenticode-signed. Windows may identify it as an unknown publisher. It has not been independently security-audited. Do not replace your established password manager until you have tested recovery with dummy records.

## Bring your existing vault into Windows

1. In your existing Veylo Chrome extension or Android app, unlock the vault and export an **encrypted `.vvlt` backup**.
2. Keep that backup and its original unlock credentials. Do not uninstall the existing app or remove its extension.
3. On the fresh Windows welcome screen, click **Restore encrypted backup**, choose the `.vvlt`, and enter its existing master passphrase and unlock word(s) or legacy key.
4. Verify several records, then save another encrypted backup from **Security & backup**.

For an additional vault, use the top **Create new vault** control, then choose **Restore encrypted backup** on the new vault's welcome screen. Desktop and Chrome store their vaults separately. Importing a backup does not establish automatic synchronization. Android NFC YubiKey-protected `VVYK` backups cannot be opened by this Windows build; retain the Android app for those vaults.

## What is included

- Create, unlock, rename and switch independent local vaults.
- Logins and Important Notes, with copy controls.
- Password generator within the login editor: length 5–50 and character options.
- Search, groups and favorites.
- Up to ten encrypted previous versions per entry; restore earlier values.
- Recently Deleted with restoration and explicitly confirmed permanent erasure.
- Local password-health heuristics, without online breach queries.
- Import from supported CSV and 1Password export formats; reviewed merges.
- CSV export with credential verification; encrypted `.vvlt` backups.
- Custom profile image, background color, and readable text contrast.
- Local Wi-Fi pairing and sync with Veylo Android 0.28.0 or newer, using the existing record and encrypted transport protocols.
- Idle lock after 60 seconds, lock on minimization and Windows session lock/suspend.
- Clipboard value clearing after 30 seconds if unchanged. Clipboard history and other applications can retain copies.

Vault encryption and backup format are inherited without alteration: Argon2id (64 MiB, 3 passes, 4 lanes), AES-256-GCM, schema-2 records. The single random word has only 7,776 possibilities and does not replace a strong unique master passphrase. There is no password reset.

## Local Wi-Fi sync

1. Put the phone and PC on the same trusted private network and unlock both vaults.
2. In Windows Veylo, choose **Local Wi-Fi sync → Pair phone**.
3. On Android, open **Settings → Local Wi-Fi sync**, scan the Windows QR, and approve the matching six-character code on both screens.
4. Reopen Local Wi-Fi sync, choose **Sync via local Wi-Fi**, scan the fresh QR, review the proposed changes on both devices, and approve.
5. Wait for the verified **In sync** result. If interrupted, repeat and review the proposed merge.

The relay is built into the EXE. If Windows Firewall prompts, allow Veylo on **Private networks only**. Guest Wi-Fi isolation, VPN routing, or a blocked firewall can prevent connection. A wired PC can participate when it shares the phone's private LAN. This is local-network sync, not cloud sync or QR-over-USB. Keep both apps open and active; their idle locks still apply.

Pairing settings are local to each vault. If the phone is already paired to a different Veylo client, it may require forgetting that pairing and pairing again. This release does not provide simultaneous automatic three-way sync between Android, Chrome, and Windows.

The original encrypted handshake and record merge remain in the renderer. The bundled relay handles only bounded opaque network frames. Production listeners accept only private IPv4 peers; local tests use an isolated loopback fixture. Physical phone-to-Windows interoperability has not been verified for this new desktop host.

## Platform differences

Chrome website autofill and floating account suggestions remain in the Chrome extension. The Windows app uses copy controls and independent storage. Android biometrics, NFC YubiKey, camera OCR and Bluetooth keyboard Auto-Type remain Android features. Windows Hello and desktop Auto-Type are not implemented. Bluetooth data sync is intentionally not exposed in this desktop test build; the earlier discovery problem is not claimed as fixed.

## Data, updates and uninstall

Encrypted vault records, encrypted pairing settings and unencrypted appearance preferences are stored in `%APPDATA%\VeyloDesktop\vaults.json`. This fixed location is independent of the installation folder. Keep encrypted `.vvlt` backups outside it. Profile names/images are visible while locked and excluded from `.vvlt` backups.

Close Veylo before updating or uninstalling. Uninstall through Windows Installed Apps or the installed `Uninstall-Veylo.exe`. Uninstall preserves vault data. It removes only known application files and shortcuts, not arbitrary files in the installation folder. A later installation uses the same data location. Never delete the data folder unless you deliberately want to remove those local vaults and have verified backups.

## Validation and limits

Passed: real Argon2id/AES-GCM backup round trip; wrong credentials and tampered file rejection; generator options and bounds; durable serialized storage and corruption refusal; desktop bridge contracts; private-address filtering, bounded framing, fragmented packets, invalid packets, a real TCP loopback relay transfer; main-process host permission/IPC/resource checks using Electron stubs; production history/edit/group/trash/restore/typed-erase controller actions using DOM stubs; JavaScript syntax; Windows x64 executable packaging and installer extraction verification.

Not verified: rendered Electron UI, actual Windows installation/uninstallation, Windows Firewall prompts, or a physical Nokia sync session. The local graphical smoke test was blocked by the execution environment's socket restrictions before the application loaded. Stub/controller tests are not a substitute for a real desktop test. This is a test build, not a security certification.

## Build from source

Node.js and npm are build-time requirements only. On Windows:

```
npm ci
npm test
node tests/feature-ui-test.mjs
npm run build:win
```

Pinned Electron and electron-builder versions are in `package-lock.json`. The standard Windows build creates an NSIS setup. The delivered cross-built installer was produced by `npm run pack:win` followed by `python3 build-installer.py <native-makensis-path>` with `NSISDIR` set to that NSIS distribution root. That script generates a normal per-user NSIS installer/uninstaller without needing to run Windows binaries on the build host. Its generated script is not needed in source control.

No signing certificate or production credentials are included. Runtime code loads bundled local assets only, with sandboxing, context isolation, disabled Node integration in the UI, restricted IPC, blocked external navigation, and denied renderer permission requests.
