# Instagram SSL Pinning Bypass

Frida-based Android security research tooling for authorized Instagram SSL pinning assessment, decrypted HTTPS traffic analysis, and mobile application security testing. The project instruments the Instagram networking stack to help researchers inspect plaintext requests and responses through the Tigon layer in a controlled, rooted test environment.

> **Authorized use only:** Use this project only on applications, devices, accounts, and networks for which you have explicit permission.

## Tested Version

The verified test environment for this repository is:

- **Target package:** `com.instagram.android`
- **Instagram version:** `449.0.0.0.74`
- **Android:** `13` / API `33`
- **Architecture:** `arm64-v8a`
- **Device:** Rooted Android test environment
- **Frida:** `17.16.4` client and server
- **Verified result:** `140–154` HTTP `200` responses and `0` pin verification failures during testing

## Compatibility

| Component | Verified value | Notes |
| --- | --- | --- |
| Target package | `com.instagram.android` | Instagram Android application |
| Instagram version | `449.0.0.0.74` | Tested release |
| Android | `13` / API `33` | Tested platform |
| CPU architecture | `arm64-v8a` | Tested device ABI |
| Device | Rooted Android test environment | Required for the documented Frida workflow |
| Frida client/server | `17.16.4` | Keep client and server versions aligned |
| Main scripts | `ig_pin_bypass.js`, `ig_dump.js` | Existing repository scripts |
| Verified capture path | Frida/Tigon plaintext capture | Captures decrypted request and response data |

Compatibility with other Instagram releases, Android versions, architectures, or Frida versions is not guaranteed. Instagram updates may change native libraries, symbols, certificate-verification behavior, or networking internals.

## What This Project Does

Android certificate pinning can prevent an application from accepting certificates presented by an authorized HTTPS inspection proxy. This project provides a Frida-based research workflow for examining that behavior in Instagram Android.

In practical terms, mobile security researchers can use the existing scripts to:

- Assess certificate-pinning behavior in a controlled test environment.
- Observe decrypted/plaintext Instagram requests and responses through the Tigon networking layer.
- Study how the application’s native TLS and networking components behave during testing.
- Support authorized Android reverse engineering and HTTPS traffic analysis.

This repository is documentation and tooling for security research. It is not intended for unauthorized access, credential collection, session theft, or interception of other users’ data.

## Features

- Frida-based Instagram Android SSL pinning research workflow.
- Plaintext request and response capture through the Tigon layer.
- Support for authorized Android HTTPS traffic analysis.
- Identification of the native TLS stack as **fizz** with Meta/Retina certificate verification.
- Main networking stack identified as **Tigon**.
- Tested with Frida `17.16.4` on Android 13 / API 33 and `arm64-v8a`.
- Focused workflow for mobile application security testing and Android reverse engineering.

## Requirements

- A rooted Android device used exclusively for authorized testing.
- Instagram Android package `com.instagram.android` installed in the test environment.
- Frida client and Frida Server version `17.16.4`.
- Frida Server matching the device architecture: `arm64-v8a`.
- Android platform tools and USB debugging.
- A workstation capable of running the Frida command-line tools.
- Written authorization from the application owner or a permitted security-testing program.

## High-Level Architecture

```text
+----------------------+       +------------------------+
| Rooted Android       |       | Frida 17.16.4          |
| Android 13 / API 33  | <---> | Client + Server        |
| arm64-v8a            |       +-----------+------------+
+----------+-----------+                   |
           |                               |
           v                               v
+----------------------+       +------------------------+
| Instagram Android    | ----> | Existing Frida scripts |
| com.instagram.android|       | ig_pin_bypass.js       |
| 449.0.0.0.74         |       | ig_dump.js             |
+----------+-----------+       +-----------+------------+
           |                               |
           v                               v
+----------------------+       +------------------------+
| Tigon networking     | ----> | Plaintext request and  |
| Native fizz TLS      |       | response capture       |
| Meta/Retina verify   |       +------------------------+
+----------------------+
```

## Usage

Start Frida Server on the authorized rooted Android test device, connect the device over USB, and verify that Frida can see it:

```bash
frida-ps -U
```

Launch the tested package with both existing scripts and write captured output to a log file:

```bash
frida -U -f com.instagram.android -l ig_pin_bypass.js -l ig_dump.js -o ig_traffic.log --runtime=v8
```

Exercise only approved application flows in the test environment. Review `ig_traffic.log` for the captured output and redact sensitive values before storing, sharing, or publishing any results.

## Traffic Capture

The verified capture method is **Frida/Tigon plaintext request and response capture**. During testing, the scripts instrument the application’s networking path so researchers can inspect decrypted traffic as it passes through the Tigon layer.

The native TLS stack identified during testing is **fizz**, with Meta/Retina certificate verification associated with the application’s TLS behavior. The tested result produced `140–154` HTTP `200` responses and `0` pin verification failures.

Results may differ when the application version, Android release, device architecture, Frida version, or native implementation changes.

## Burp Suite

Burp Suite can be useful for authorized Android HTTPS traffic analysis and for validating proxy reachability in a controlled lab. However, the verified result documented here is **Frida-based decrypted/plaintext traffic capture through Tigon**.

This repository does **not** claim that native Burp transparent MITM interception is a completed or verified feature. Native Burp interception may be considered an optional future enhancement and can depend on Android trust configuration, proxy settings, application behavior, and changes to Instagram’s networking implementation.

## Compatibility and Version Updates

Instagram releases can change frequently. A workflow verified against `449.0.0.0.74` should not be assumed to work on another release without testing.

When evaluating a new version, record at minimum:

1. Exact Instagram version.
2. Android version and API level.
3. Device architecture.
4. Frida client and server versions.
5. Whether both scripts load successfully.
6. Whether pin verification failures occur.
7. Whether plaintext Tigon capture remains available.
8. The observed request and response results.

Do not use the phrase “latest version” for an unverified release; use the exact version number tested instead.

## Troubleshooting

### `frida-ps -U` does not show the device

- Confirm that `adb devices` lists the Android device as authorized.
- Verify USB debugging is enabled.
- Confirm Frida Server is running on the device.
- Check that the Frida client and server are both `17.16.4`.
- Confirm the Frida Server binary matches `arm64-v8a`.

### Frida cannot spawn Instagram

- Confirm the package name is `com.instagram.android`.
- Close stale Instagram processes and retry.
- Verify the device is rooted and Frida Server has the required permissions.
- Confirm that the installed Instagram version matches the tested release `449.0.0.0.74`.
- Review Frida diagnostic output for architecture or runtime errors.

### No plaintext traffic appears in the log

- Confirm both `ig_pin_bypass.js` and `ig_dump.js` are present and loaded.
- Use the verified launch command exactly as shown above.
- Exercise approved application flows after the process starts.
- Confirm the target release, Android version, architecture, and Frida versions match the tested environment.
- Check that the application has not changed its Tigon or native TLS implementation.

### The application exits or requests fail

- Test with a clean, disposable rooted device environment.
- Confirm the scripts are being used with the tested Instagram release.
- Review Frida output and Android diagnostic logs.
- Re-check the Frida Server architecture and version.
- Stop testing if the activity falls outside the scope of written authorization.

## Changelog / Tested Versions

### `1.0.0` — Verified research baseline

- Tested Instagram Android `449.0.0.0.74`.
- Tested on Android `13` / API `33`.
- Tested on `arm64-v8a` rooted Android hardware.
- Tested with Frida client/server `17.16.4`.
- Verified Frida/Tigon plaintext request and response capture.
- Observed `140–154` HTTP `200` responses and `0` pin verification failures.
- Identified the native TLS stack as fizz with Meta/Retina certificate verification.
- Identified Tigon as the primary networking stack.

## Demo and Screenshots

Add screenshots or recordings only from an authorized lab environment. Redact credentials, cookies, access tokens, private messages, personal information, and other sensitive data.

- `[Screenshot placeholder: Frida session using the verified launch command]`
- `[Screenshot placeholder: sanitized ig_traffic.log output]`
- `[Screenshot placeholder: Tigon plaintext request and response capture]`
- `[Demo placeholder: authorized Android security-testing workflow]`

## Private / Commercial Releases

For authorized private research, commercial testing, or compatibility work, contact the maintainer:

[![Telegram](https://img.shields.io/badge/Telegram-Contact%20%40jalwan0-26A5E4?logo=telegram&logoColor=white)](https://t.me/jalwan0)

## Authorized Security Testing Disclaimer

This project is provided for authorized security testing, defensive research, debugging, education, and Android application analysis only. Do not use it to access accounts, intercept traffic, collect credentials, extract sessions, monitor other users, or bypass security controls without explicit permission.

You are responsible for complying with applicable laws, contracts, privacy requirements, platform terms, and security-testing program rules. Use isolated test accounts, dedicated devices, and sanitized logs. Do not test production users or unrelated services.

## Relevant Topics and SEO Keywords

Instagram SSL Pinning Bypass · Instagram Android SSL Pinning · Frida Instagram · Frida SSL Pinning Bypass · Android SSL Pinning Testing · Android HTTPS Traffic Analysis · Burp Suite Android Traffic Analysis · Instagram Network Traffic Analysis · Mobile Application Security Testing · Android Reverse Engineering
