# ToDoPro macOS updates

This repository is the public Sparkle appcast and release feed for ToDoPro.
The feed is intentionally empty until the first signed, notarized macOS release is published.

## Publishing a release

1. Build the macOS app with a monotonically increasing `CFBundleShortVersionString` and `CFBundleVersion`.
2. Archive and export using **Developer ID Application** signing. App Store/TestFlight builds are not valid Sparkle payloads.
3. Notarize the exported app or DMG with Apple, wait for acceptance, and staple the notarization ticket.
4. Verify the distributed artifact opens on a clean Mac and that its signature and notarization validate.
5. Use the Sparkle tools that match the app's bundled Sparkle version to generate the update archive, EdDSA signature, and file length. Keep the EdDSA private key only in the macOS Keychain under the account `com.hiyuno.todoproapp`; never put it in this repository, source code, CI logs, or release assets.
6. Upload the signed, notarized artifact to a GitHub Release and add an item to `appcast.xml` containing the exact download URL, version, build number, byte length, and Sparkle EdDSA signature.
7. Validate the XML and the published enclosure before announcing the update.

## Security rules

- Never commit private keys, API tokens, certificates, provisioning profiles, or unredacted CI logs.
- Never reuse a lower build number. Sparkle clients must see strictly increasing builds.
- The appcast must point only to HTTPS artifacts from the intended GitHub repository.
- Preserve the public EdDSA key in the ToDoPro app configuration; only the private signing key belongs in the local Keychain.
- Review every appcast change before publishing it.

The canonical appcast URL is:

`https://raw.githubusercontent.com/hiyuno/todopro_updates/main/appcast.xml`
