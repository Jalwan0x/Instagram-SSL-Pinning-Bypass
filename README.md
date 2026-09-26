# Instagram Android SSL Pinning Testing Utility

Frida-based utility documentation for **authorized Android mobile application security testing**, SSL/TLS pinning assessment, and HTTPS traffic analysis through Burp Suite. This repository is intended for researchers, mobile application testers, and developers validating certificate-pinning behavior in controlled environments.

> **Tested target:** Instagram Android `448.0.0.52.84`  
> **Build:** `385412061`  
> **Platform:** Android  
> **Architecture:** `arm64-v8a`

## Overview

Certificate pinning can prevent an Android application from accepting certificates issued by an HTTPS interception proxy. During an authorized assessment, Frida can be used to instrument the target process at runtime so that a tester can observe application behavior and analyze permitted HTTPS traffic in Burp Suite.

This project focuses on **Instagram SSL pinning testing**, **Frida SSL pinning research**, **Android traffic analysis**, and **Burp Suite HTTPS interception** for legitimate mobile application security testing. It does not provide credential theft, session theft, account access, or unauthorized interception functionality.

## High-Level Workflow

```text
+------------------+     +------------------+     +------------------+
| Rooted Android   | --> | Instagram App   | --> | Frida / Frida    |
| test device      |     | under test      |     | Server runtime   |
+------------------+     +------------------+     +------------------+
                                                            |
                                                            v
                                                   +------------------+
                                                   | Burp Suite       |
                                                   | proxy listener   |
                                                   +------------------+
                                                            |
                                                            v
                                                   HTTPS traffic
                                                   analysis
```

Typical flow:

1. Prepare a dedicated, rooted Android test device or emulator.
2. Configure the device to use the Burp Suite proxy and install the Burp CA certificate in the authorized test environment.
3. Start Frida Server on the device.
4. Launch Instagram and attach the existing repository script using Frida.
5. Confirm permitted HTTPS requests are visible in Burp Suite for analysis.

## Compatibility

| Component | Tested / expected value | Notes |
| --- | --- | --- |
| Target application | Instagram Android `448.0.0.52.84` | Tested application version |
| Build | `385412061` | Associated target build |
| Android platform | Android | Use a dedicated authorized test device |
| CPU architecture | `arm64-v8a` | Frida Server and tooling must match the device ABI |
| Device privileges | Root required | Required for the documented runtime instrumentation workflow |
| Instrumentation | Frida + Frida Server | Keep client and server versions compatible |
| Proxy tooling | Burp Suite | Used for HTTPS interception and traffic analysis |

Compatibility with other Instagram releases, Android versions, architectures, or Frida releases is not guaranteed. Application updates may change certificate-pinning behavior, native libraries, process names, or runtime control flow.

## Requirements and Prerequisites

- A rooted Android device or emulator used only for authorized testing.
- Instagram installed from a legitimate source and configured for a permitted test account or test environment.
- Frida client installed on the analysis workstation.
- A matching **Frida Server** binary for the device architecture (`arm64-v8a`).
- Burp Suite installed on the analysis workstation.
- USB debugging and Android platform tools, including `adb`.
- A network configuration that allows the Android device to reach the workstation running Burp Suite.
- Permission from the application owner or an applicable security-testing program before testing.

### Verify the Device Connection

```bash
adb devices
adb shell getprop ro.product.cpu.abilist
```

Confirm that the device is recognized and reports an architecture compatible with the Frida Server binary you intend to use.

### Start Frida Server

Place the matching Frida Server binary on the device, make it executable, and start it using your approved device administration workflow. The exact deployment command can vary by device image, root solution, and Frida release.

Verify connectivity from the workstation:

```bash
frida-ps -U
```

Keep the Frida client and Frida Server versions aligned whenever possible. Consult the official Frida documentation for version-specific installation and device setup guidance.

## Installation

1. Clone or download this repository into your authorized research workspace.
2. Review the existing repository scripts before running them.
3. Install a Frida client on the workstation.
4. Deploy the matching Frida Server to the rooted Android device.
5. Configure Burp Suite as described below.
6. Connect the device over USB or an approved network transport.
7. Confirm `frida-ps -U` lists processes successfully.

This README intentionally documents the testing workflow without reproducing or generating bypass implementation code.

## Usage

Use the **existing scripts included in this repository** with Frida according to their filenames and documented entry points. Do not assume that a script written for one Instagram release is compatible with another release.

A typical authorized invocation pattern is:

```bash
frida -U -f <authorized-package-name> -l <existing-script>.js
```

Depending on the script and Frida version, you may instead need to attach to an already-running process. Review the script comments and validate behavior on a disposable test device before collecting traffic.

### Recommended Testing Procedure

1. Close Instagram on the test device.
2. Start Burp Suite and verify the proxy listener is reachable from the device.
3. Start Frida Server.
4. Launch or attach to the application with the existing repository script.
5. Exercise only approved application flows.
6. Capture and analyze HTTPS requests in Burp Suite.
7. Avoid collecting real user credentials, session tokens, private messages, or unrelated personal data.
8. Remove test certificates, proxy settings, and instrumentation artifacts when testing is complete.

## Burp Suite Configuration Overview

1. Open **Proxy → Proxy settings** in Burp Suite.
2. Confirm an HTTP proxy listener is bound to an address reachable by the Android device, such as the workstation's LAN address.
3. Note the listener port, commonly `8080`, and use the actual configured port in the device proxy settings.
4. Configure the Android Wi-Fi network to use the workstation IP address and Burp listener port.
5. Install the Burp CA certificate only on the authorized test device or emulator.
6. Confirm that Burp receives a harmless test request before launching the target application.
7. Use **Proxy → HTTP history** and **Logger** for approved HTTPS traffic analysis.

Network reachability, Android certificate trust behavior, and application security controls can vary by OS version and device configuration. Do not weaken security controls on production devices or networks.

## Troubleshooting

### `frida-ps -U` does not show the device

- Confirm `adb devices` lists the device as authorized.
- Reconnect the USB cable and accept the device authorization prompt.
- Verify USB debugging is enabled.
- Check that the Frida Server process is running with appropriate privileges.
- Confirm the Frida client and server versions are compatible.

### Frida cannot spawn or attach to the application

- Verify the package name and process name for the installed application version.
- Confirm the device is rooted and the Frida Server has the required permissions.
- Close stale application processes and retry.
- Check whether the installed app build differs from the tested build `385412061`.
- Review Frida output for architecture or gadget/server mismatch errors.

### Burp shows no traffic

- Confirm the Android proxy points to the correct workstation IP and listener port.
- Ensure the workstation firewall permits connections to the Burp listener.
- Verify the device and workstation are on the same approved network path.
- Test with a browser or other permitted application to confirm proxy reachability.
- Check that the target process was launched or attached using the existing script.

### TLS requests fail or the application exits

- Confirm the script matches the tested Instagram release.
- Re-check the Frida Server architecture (`arm64-v8a`) and version.
- Review Logcat and Frida diagnostic output.
- Test with a clean, disposable device snapshot.
- Do not attempt to defeat protections outside the scope of your written authorization.

### Instagram updated

Instagram releases can change frequently. A script tested against `448.0.0.52.84` may stop working after an application update. Record the application version, build number, Android version, device model, Frida version, and observed behavior before reporting compatibility.

## Tested Versions and Changelog

### Tested release

- Instagram Android: `448.0.0.52.84`
- Build: `385412061`
- Platform: Android
- Architecture: `arm64-v8a`

### Changelog

#### 1.0.0

- Added documentation for authorized Instagram Android SSL pinning testing.
- Documented the tested application version and build.
- Added Frida, Frida Server, Burp Suite, compatibility, setup, usage, troubleshooting, and safety guidance.

Future compatibility notes should include the exact Instagram version, build number, Android version, device architecture, Frida client/server versions, and test result.

## Demo and Screenshots

Screenshots and demonstrations should be added only from an authorized lab environment and must not expose credentials, cookies, access tokens, private messages, personal information, or other sensitive data.

- `[Screenshot placeholder: Frida Server running on an authorized arm64-v8a test device]`
- `[Screenshot placeholder: Burp Suite proxy listener configuration]`
- `[Screenshot placeholder: Burp Suite HTTPS traffic analysis view with sensitive values redacted]`
- `[Demo placeholder: sanitized test workflow recording]`

## Responsible Use and Disclaimer

This project is provided for **authorized security testing, defensive research, debugging, and education only**. Use it only on applications, devices, accounts, and networks for which you have explicit permission or a clearly applicable security-testing authorization.

You are solely responsible for complying with all applicable laws, contracts, platform terms, privacy requirements, and program rules. Do not use this project to access accounts, intercept traffic, collect credentials or session tokens, bypass security controls on systems you do not own, or target unsuspecting users. The maintainers are not responsible for misuse, damage, data loss, privacy violations, or legal consequences resulting from use of this repository.

## Private or Commercial Releases

For authorized private or commercial research releases, consulting, or customized compatibility work:

- Telegram: `@your_telegram_handle`

Replace the placeholder above with the maintainer's verified contact information before publishing a commercial offering.

## Search Topics

Instagram SSL pinning bypass · Instagram Android SSL pinning · Frida SSL pinning · Android SSL pinning testing · Burp Suite HTTPS interception · Android traffic analysis · mobile application security testing
