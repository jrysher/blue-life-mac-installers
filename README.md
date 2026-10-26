# Blue Life Mac installers

Small deployment packages for Apple Business built-in device management. No extra paid hosting or device management service is used.

## ChatGPT bootstrap 1.0.0

The package installs the official ChatGPT desktop app on Apple silicon Macs. It contains only the source installation script in `scripts/postinstall`; it does not redistribute the OpenAI application.

At install time it downloads the current app from the URL linked by [OpenAI's desktop documentation](https://learn.chatgpt.com/docs/app). The official download currently uses the filename `Codex.dmg`, the app name `ChatGPT.app`, bundle ID `com.openai.codex`, and OpenAI signing team `2DC432GLL2`.

The script verifies the app's code signature, bundle ID, and signing team before copying it into `/Applications`. It leaves an existing valid app in place and fails if an existing app does not match the expected identity. It uses the original vendor signature and grants no privacy permissions. Employee sign-in remains interactive.

### Deploy in Apple Business

- URL: `https://github.com/jrysher/blue-life-mac-installers/releases/download/v1.0.0/bluelife-chatgpt-bootstrap-1.0.0.pkg`
- SHA-256: `e838acba7367b7d0fc80e60290a1d8bcc021063d89a16a49f64b8fdf4482aed5`
- Bundle ID: `com.openai.codex`
- Package version: `1.0.0` (the app version is determined by OpenAI's current download)

Add the package to an employee Blueprint. Test installation on the first managed Mac before broader rollout. This is a custom unsigned wrapper; the downloaded vendor application retains its original signature. A signature or identity mismatch causes installation to stop.

The release URL and package hash are immutable for this version. Publish a new version for changes. If OpenAI changes the download path, app name, bundle ID, or signing team, review the new identity and publish an updated package.

The source and package contain no credentials, employee information, serial numbers, or company documents.

### Rebuild

```sh
chmod 755 scripts/postinstall
pkgbuild --nopayload --scripts scripts --identifier com.bluelifesecurity.chatgpt.bootstrap --version 1.0.0 bluelife-chatgpt-bootstrap-1.0.0.pkg
shasum -a 256 bluelife-chatgpt-bootstrap-1.0.0.pkg
```

No OpenAI endorsement is implied. ChatGPT and its application remain the property of OpenAI; employees use their existing authorized accounts.
