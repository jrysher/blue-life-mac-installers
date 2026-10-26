# Blue Life Mac installers

Small deployment packages for Apple Business built-in device management. No extra paid hosting or device management service is used.

## ChatGPT bootstrap 1.0.1

The package installs the official ChatGPT desktop app on Apple silicon Macs. It contains only the source installation script in `scripts/postinstall`; it does not redistribute the OpenAI application.

At install time it downloads the current app from the URL linked by [OpenAI's desktop documentation](https://learn.chatgpt.com/docs/app). The official download currently uses the filename `Codex.dmg`, the app name `ChatGPT.app`, bundle ID `com.openai.codex`, and OpenAI signing team `2DC432GLL2`.

The script verifies the app's code signature, bundle ID, and signing team before copying it into `/Applications`. It leaves an existing valid app in place and fails if an existing app does not match the expected identity. It uses the original vendor signature and grants no privacy permissions. Employee sign-in remains interactive.

### Deploy in Apple Business

- URL: `https://github.com/jrysher/blue-life-mac-installers/releases/download/v1.0.1/bluelife-chatgpt-bootstrap-1.0.1.pkg`
- SHA-256: `50509b24f55e7e08b6932b70a8b07f672bbdb726d85cb7836530925fd3e8660c`
- Bundle ID: `com.openai.codex`
- Package version: `1.0.1` (the app version is determined by OpenAI's current download)

Add the package to an employee Blueprint. Test installation on the first managed Mac before broader rollout. Version 1.0.0 was unsigned and must not be used for automatic deployment. Version 1.0.1 is a distribution package signed by the company installer certificate. Distribute and verify trust in the public certificate using an Apple Business Certificate configuration before assigning this installer. The downloaded vendor application retains its original signature. A signature or identity mismatch causes installation to stop.

The release URL and package hash are immutable for this version. Publish a new version for changes. If OpenAI changes the download path, app name, bundle ID, or signing team, review the new identity and publish an updated package.

The source and package contain no credentials, employee information, serial numbers, or company documents.

### Signing and review

[Apple documents the free company certificate approach](https://support.apple.com/en-gb/guide/business/axm20c32e0c6/web). The release includes only the public certificate; private signing material is never included. Certificate fingerprint (SHA-256): `E4845C3DDAD28AAA47F7993DBC0F7995372991F67FBA3B9E4E8C4BDCB789DDD2`; expires October 26, 2027. Rotate and re-sign before expiry. Anyone holding the private key can sign installer packages trusted by the Macs that receive this certificate. Remove its configuration if compromised.

The bootstrap is packaged using `pkgbuild --nopayload` and wrapped using `productbuild --package`. The distribution archive carries a standard RSA XAR signature. macOS independently recognizes the signer as the company certificate and reports it as untrusted on Macs without the certificate configuration. Archive extraction and script comparison passed. **Live automatic installation remains unverified until the first managed Mac reports success.**

No OpenAI endorsement is implied. ChatGPT and its application remain the property of OpenAI; employees use their existing authorized accounts.
