# Security policy

Veylo is currently test software. The published builds have automated checks but have not completed independent security auditing or broad physical-device validation.

## Reporting

Do not post real passwords, vault backups, unlock words, pairing QR contents, private keys or personally identifying information in public issues. Reproduce problems with dummy records whenever possible.

If GitHub displays **Report a vulnerability** in this repository's Security tab, use that private channel for a sensitive report. Otherwise, open a content-free issue requesting a private reporting channel before sharing technical exploit details or private data. Do not assume a private channel has been configured if it is not visible.

No response-time or remediation-time commitment is published for these test builds.

## Current boundaries

- Standard vaults use AES-256-GCM and Argon2id. A strong, unique master passphrase remains essential.
- A single unlock word has 7,776 possible values and is not a substitute for a high-entropy password.
- Unlocking exposes the requested vault data to the application and operating system. Veylo cannot guarantee secrecy on a compromised device.
- CSV exports are readable; clipboard history and destination applications can retain copies.
- NFC YubiKey-protected Android backups cannot be unlocked by the released desktop/Chrome apps.
- Local sync, backups and application updates require compatible record formats and deliberate testing. Sync is not an independent backup.
- Windows packages are unsigned. Verify checksums against the release; checksums establish byte identity, not a security audit or publisher certificate.
- Bluetooth data discovery remains unresolved. Windows-to-Chrome Veylo Link and Forge Sentinel integration are not included in the listed release.
