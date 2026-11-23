# Veylo — Feature reference

Veylo is a password manager for organizing encrypted logins and important information, with platform-specific tools for using that information deliberately. The name draws on **veil and protection**: credentials remain covered until the user chooses to unlock, reveal, copy or fill them. Created by **ElctroZeus**, presented under **CodeLineCore**.

This document describes **Android 0.28.0, Chrome 0.28.0 and Windows 0.29.0**. “Implemented” means present in these test builds; it does not imply that every device, browser form or connection has been validated. Veylo Link is separately identified as work in progress.

## Vaults and protection

### 1. Encrypted local vault storage — all platforms

Passwords and secure notes are stored inside an encrypted vault rather than a readable credentials list. The existing format uses **AES-256-GCM**, which provides confidentiality and detects changes to authenticated encrypted data. Veylo is designed for local storage; these releases do not upload vaults to a cloud account.

### 2. Argon2id key derivation — all platforms

Veylo derives the key used to protect the vault key from the user's credentials using Argon2id, configured for **64 MiB of memory, three passes and four lanes**. This makes password guessing more expensive, but cannot compensate for a weak or reused master passphrase. New vaults require a master passphrase of at least 16 characters.

### 3. Master passphrase plus a random unlock word — all platforms

New vaults combine a master passphrase with a generated unlock word. Both are required for normal credential-based unlocking and restoration. Earlier six-word secrets and legacy keys remain supported where the backup format permits. A single word has only 7,776 possibilities; it is an additional secret, not a replacement for a strong master passphrase or equivalent to a hardware key.

### 4. Controlled reveal of unlock fields — all platforms

Show/Hide controls let the user check sensitive unlock fields without leaving them permanently visible. Short reveal windows reduce accidental exposure while entering or recording credentials. Revealing a value still makes it visible to anyone who can see the screen.

### 5. Unlock-word rotation — all platforms

After verifying the current credentials, the user can change an existing vault to a newly generated unlock word. The change affects that local vault and future backups. Existing backups retain their original protection and still require their old credentials.

### 6. Multiple independent vaults — all platforms

Separate vaults allow work and personal information to use different unlock credentials. Vault switching changes which encrypted collection is opened; it is not merely a filter within one shared vault. Platform-local vaults do not become synchronized automatically because their names match.

### 7. Manual and automatic locking — all platforms

The user can explicitly lock the vault when finished. The test builds also lock after 60 seconds of inactivity and on relevant app or window lifecycle changes. Android locks when the app leaves the foreground; Chrome locks hidden/closed vault views; Windows adds minimization, session-lock and suspend handling. Active sync sessions are closed when the vault locks.

### 8. Protected vault removal — Android

Android places whole-vault removal under the selected vault's additional options. The operation requires its unlock credentials and a typed confirmation, helping prevent accidental removal. This is different from deleting one entry. Whole-vault deletion is not implemented as an equivalent control in the released Windows and Chrome interfaces.

## Logins, notes and organization

### 9. Structured login records — all platforms

Each login can hold a title, username, password, website and supporting notes. Keeping these fields together gives the user context when selecting an account, particularly when several accounts belong to the same service. A saved website address also supports matching in the Chrome filling workflow.

### 10. Important Notes — all platforms

Secure notes store reusable text such as addresses, reference details and instructions within the encrypted vault. Notes have their own browsing and editing flow, so non-login information can be found without inventing a username or password for it.

### 11. Field-specific copy controls — all platforms

Copy actions transfer only the chosen username, password or note to the system clipboard. This supports forms and applications that cannot use Veylo autofill. Clipboard history and other applications can retain copied values, so copying is an intentional disclosure outside the vault.

### 12. Conditional clipboard clearing — Windows

Windows Veylo schedules a copied value for clearing after 30 seconds. It clears the clipboard only if it still contains that same value, avoiding removal of newer content copied by the user. This does not erase Windows clipboard history or copies already captured by another application.

### 13. Search — all platforms

Vault search narrows the visible list using identifying information such as titles, usernames, websites and group labels. It is intended to make an account easier to locate without exposing every password on the list. Exact searchable fields vary by platform.

### 14. Groups — all platforms

A record can be assigned a group such as Work, Personal or Banking, then located through group filtering. Groups organize records within one vault; they do not introduce separate encryption keys or access permissions. Use independent vaults when different credentials are needed.

### 15. Favorites — all platforms

Frequently used entries can be marked as favorites and shown through a dedicated filter. Favorite status belongs to the record and is included in the supported record synchronization format.

### 16. Password generator — all platforms

The generator is available within the login editor rather than as a separate vault-menu destination. It creates passwords using cryptographically secure random input, avoiding predictable selection from ordinary pseudo-random UI functions. Generated passwords can be inserted into a record or copied for use elsewhere.

### 17. Generator length and character controls — all platforms

Users can select a length from **5 to 50 characters** and choose numbers, letters, special characters, and mixed uppercase/lowercase letters. Mixed case applies when letters are enabled, and generated results include the selected character groups. The five-character minimum supports restrictive legacy forms; it is not a recommended security target.

### 18. Entry history — all platforms

Edits preserve up to **ten previous versions** of a login or note inside the encrypted vault. History allows inspection and restoration of earlier values, including organizational metadata. Restoring an earlier version also preserves the current values as another history entry, subject to the retention limit. History cannot recover changes made before it was introduced.

### 19. Recently Deleted — all platforms

Deleting a record initially moves it into a recoverable state. It is excluded from normal browsing, filling, health checks and active-record CSV exports, while remaining available for restoration. There is no automatic deletion expiry in these builds.

### 20. Explicit permanent entry erasure — all platforms

Permanent erasure requires typing `ERASE`. It removes the entry's fields and history from the current vault and retains a minimal encrypted deletion marker to prevent an older synchronized copy of the same record from silently restoring it. Separate backups and independently duplicated records are unaffected; this is not a claim of physical storage sanitization.

### 21. Offline password-health review — all platforms

Local checks identify reused passwords, missing passwords, short values, repeated characters and a limited set of common patterns. Results help prioritize review without sending passwords to a breach-search service. These heuristics are not a complete strength measurement and do not prove that an unflagged password is uncompromised.

## Android-specific access and capture

### 22. Biometric unlocking — Android

On supported devices, the user can enable strong biometric authentication to unlock a standard vault without typing its passphrase and unlock word each time. Android determines which enrolled fingerprint or face sensors meet the required strength. Hardware compatibility and enrollment must be tested on the actual phone; the existence of a face-unlock setting alone does not establish support.

### 23. Optional NFC YubiKey protection — Android

Veylo can require a configured **YubiKey 5 NFC or 5C NFC**, using slot-2 HMAC-SHA1 challenge-response, in addition to the vault's credentials. Enabling or disabling this changes vault protection and requires reauthentication and key verification. When hardware protection is enabled, biometric-only unlocking is disabled. This is not generic support for every YubiKey mode, USB security key or passkey.

### 24. Native Android app autofill — Android

The Android autofill service supports deliberate selection for compatible native-app login forms. The user approves and binds an app to a saved item before using it. The released Android implementation does not include browser/website autofill or automatic capture of newly typed passwords.

### 25. Photo-to-text notes — Android

The note-scanning flow converts an image into editable text for an Important Note. It is useful for capturing an address, reference label or vehicle registration text without retyping it. The user reviews and corrects recognition results before saving; OCR output is not treated as verified information.

### 26. Local English and Arabic OCR — Android

Bundled recognition models support English, Arabic and a combined mode without sending the image to an online OCR service. The scanning workflow includes image review/cropping and editable output. A pending text draft must be unlocked and saved; it can expire, and the original photo is not retained as a vault attachment by this feature.

### 27. Bluetooth keyboard Auto-Type — Android

The phone can act as a Bluetooth keyboard for a paired computer and send an approved username, password or supported note text into the focused field. Controls include selecting or pairing a computer and stopping an active typing sequence. Users must focus the correct destination and use a supported keyboard layout. This keyboard function is separate from Bluetooth data synchronization.

### 28. Bluetooth connection status and setup — Android

The interface provides connection information for Auto-Type and sync, with Bluetooth-related setup grouped together. Being paired or connected as a keyboard does not establish that a compatible serial data-sync service is discoverable by Chrome.

## Chrome-specific website integration

### 29. Deliberate website filling — Chrome

From the extension's toolbar workflow, the user selects an account and chooses whether to fill a username, password or both. Veylo checks the saved HTTPS origin against the active page and rechecks the destination before filling. It does not automatically submit the form.

### 30. Opt-in floating website launcher — Chrome

For individually approved HTTPS websites, a floating launcher opens the account-selection flow. This reduces the steps needed to choose among multiple accounts for that site. Access is granted per website rather than enabling unrestricted filling on every page by default.

### 31. Exact-origin matching and restrictive form handling — Chrome

Suggestions and filling use saved website origins instead of relying only on page titles or visual similarity. Unsupported, changed, non-HTTPS, ambiguous or cross-origin-frame situations can be refused. These checks reduce some mis-filling risks; they do not certify that a website is legitimate or guarantee protection from every phishing scenario.

### 32. Website-permission management — Chrome

Users can review and remove websites approved for the floating launcher. Removing access stops the related integration on that host. Extension permissions and site trust settings remain local and are not synchronized as password records.

## Migration, backup and synchronization

### 33. Reviewed password import — all platforms

Supported inputs include 1Password 1PUX exports, CSV exports from Google Password Manager, Apple Passwords and other compatible managers, and supported ZIP containers. Records are parsed and reviewed before being added. Compatibility depends on the export's actual structure; this is not a direct connection to another vendor's account.

### 34. Import preview, duplicate handling and conflict retention — all platforms

The import process previews candidate records, skips detected duplicates and retains differing records rather than silently overwriting a password. Selected records can be added after review. Unsupported 1PUX documents, attachments, passkeys and oversized records may be skipped; extra source details may be retained as reference text rather than executable features.

### 35. CSV export — all platforms

General CSV and Bitwarden-style CSV options support migration to other applications. The Bitwarden-style format can include notes, group/folder and favorite information where supported. Exports require credential verification and contain readable data. They do not retain encrypted history, Recently Deleted or every application-specific field.

### 36. Encrypted backup and restoration — all platforms, with hardware-key limits

A `.vvlt` backup preserves supported vault records, history and deletion state in encrypted form. Restoration requires the original credentials, and hardware-protected backups require their configured NFC key on Android. Windows and Chrome cannot open Android NFC-protected `VVYK` backups. Appearance and platform-specific permissions are separate from these backups.

### 37. QR-assisted local Wi-Fi pairing — Android and Chrome/Windows

Devices on the same private network can establish a pairing using a temporary QR code and explicit approval. The QR carries connection/session material; it does not contain the user's readable password list. Pairing must be followed by the sync workflow. The current desktop and Chrome clients are separate pairing targets, not one automatically shared vault.

### 38. Encrypted local record transfer — Android and Chrome/Windows

The existing local transport uses X25519 key agreement, HKDF/HMAC-SHA-256 and AES-256-GCM in its authenticated session design. Vaults remain encrypted at rest, and the relay transports opaque frames rather than holding vault keys. Both participating vaults must be unlocked for reviewed record synchronization.

### 39. Review before merging and verified sync status — local sync

Before applying a merge, both screens show the proposed record changes. The protocol compares record digests and exchanges saved-state confirmations before reporting a successful sync. A green result describes that completed exchange; it does not mean every other device or every later edit is automatically synchronized.

### 40. Conflict preservation and deletion-aware sync — local sync

Concurrent differing edits can be retained as separate copies instead of silently choosing one. History, groups, favorites and deletion metadata travel with records. Permanent-erasure markers take precedence over stale copies of the same identity. Interruptions can leave one side saved before the other; the next exchange must be reviewed again, and independent backups remain necessary.

### 41. Chrome local-sync companion — Windows helper for Chrome

The companion package supplies the native transport needed by Chrome's local Wi-Fi sync flow. It carries bounded encrypted packets and requires no Python installation. It is different from the standalone Windows Veylo app. Chrome 0.28.0 still uses this separate helper; Windows 0.29.0 includes its own relay.

### 42. Integrated Windows local-sync relay — Windows

The standalone Windows application includes the private-network relay needed for its phone-sync flow, eliminating a separate helper installation for that app. Windows Firewall, VPN routing and network isolation can still affect connectivity. Physical phone-to-Windows validation remains required.

### 43. Bluetooth data sync — Android/Chrome, experimental and unresolved

The Android and Chrome packages include Bluetooth Classic serial/RFCOMM sync code with pairing approval and encrypted record transfer. Phone discovery has failed in the user's setup, and this route is **not considered verified or fixed**. Windows 0.29.0 does not expose Bluetooth data sync. Successful keyboard Auto-Type is not evidence that this data transport works.

## Appearance and Windows packaging

### 44. Custom vault name and locked-screen identity — all platforms

Users can name each vault and see that identity when choosing or unlocking it. The feature helps distinguish separate work and personal collections. Matching names on two devices do not establish a cryptographic link or shared storage.

### 45. Custom JPG or animated GIF profile image — all platforms

The profile area accepts supported user images, subject to format and size checks. This allows a vault to use a personal image or logo while retaining the Veylo interface. Profile imagery is an appearance preference, visible when locked, and not protected as a vault record.

### 46. Background customization and adaptive text contrast — all platforms

Users can choose a background color, and the interface adjusts foreground colors to maintain readable contrast. The default styling is minimalist black, white and gray, with Arial-oriented typography and platform fallbacks where applicable.

### 47. Standalone Windows installer — Windows 10/11 x64

The Windows release packages the desktop runtime, application and relay in an installer. End users do not need Node.js, Python or developer tooling. The test installer is unsigned, so Windows may identify an unknown publisher. Packaging checks do not substitute for a real installation test.

### 48. Persistent desktop data and conservative uninstall — Windows

Desktop data uses a stable per-user location independent of the installation folder. The uninstaller removes known application files and shortcuts while retaining local vault data. Android uninstall behavior is different and can remove its local vaults; encrypted backups should be created before any uninstall or migration.

## Work in progress — not included in these downloads

### Veylo Link

A planned local connection hub will link the Chrome extension to the Windows vault through a native host, alongside the phone's private-network connection. Intended controls include Link device, Sync now, explicit approval and verified status. Work has begun, but the released packages do **not** provide a completed Windows-to-Chrome link or automatic three-way synchronization.

### Forge Sentinel integration

Integration into the Forge Sentinel browser has been requested. These downloads remain standalone Veylo packages and do not establish that integration.

## Features discussed but not implemented in the listed releases

KDBX database compatibility; TOTP generation; passkey storage/provider support; generic OnlyKey support; file attachments; arbitrary custom fields/field references; HIBP breach lookups; XML/HTML vault exports; a public CLI; SSH agent and desktop secret-service integration; user-selectable Twofish/ChaCha20 vault encryption; Windows Hello unlocking; Windows-originated Auto-Type; online/cloud sync; and automatic website password saving.

Google Authenticator login, Google Drive/Gmail sync and QR-over-USB are not part of the current release direction. Photo scanning is OCR, not a general-purpose live AI assistant.

## Operational limits

- Existing vault limits: **4 MiB** encrypted vault size and **2,000 records**, including recovery records/deletion markers. History consumes storage.
- New schema-2 saves require compatible releases; older apps may not read updated backups.
- No independent security audit has been completed. No claim is made that Veylo is unbreakable, admin-proof or immune to a compromised operating system.
- Rendered Windows UI, physical Nokia biometric behavior, actual Windows installation and real phone-to-PC network/firewall behavior remain device-validation tasks.
- Store test data first, verify recovery, and retain an established password manager and independent encrypted backups while evaluating these builds.

## Update notices — added in 0.30.0

### 49. In-app update checking with a pulsing white border — all platforms

A persistent bottom update button is available in the main application, including its locked screen. It checks the public Veylo GitHub releases through a dedicated Cloudflare Worker and compares the version for the current platform. When a newer build is known, the button displays its version and gently pulses its white border while leaving its text steady. Reduced-motion preferences disable animation. Settings → Updates provides manual checking and an automatic-check switch. Automatic checks occur while the interface is open, at most every six hours; these are not push alerts while the application is closed. The control opens only a Veylo GitHub release page; downloading and installation remain user actions. The service receives no vault contents, though Cloudflare receives ordinary network metadata such as IP addresses. Older installations need one manual update to 0.30.0 before this feature is available.
