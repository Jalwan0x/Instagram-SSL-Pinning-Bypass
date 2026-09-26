# Instagram Android SSL Pinning Testing Utility (Frida + Burp Suite)

Professional documentation for an **authorized** Android SSL pinning testing workflow focused on Instagram traffic analysis in controlled security assessments.

This repository is intended for **mobile application security testing**, including topics commonly searched as: **Instagram SSL pinning bypass**, **Instagram Android SSL pinning**, **Frida SSL pinning**, **Android SSL pinning testing**, **Burp Suite HTTPS interception**, **Android traffic analysis**, and **mobile application security testing**.

## Authorized Use Only

This project is for **lawful, authorized security research only** (e.g., assets you own or have explicit written permission to test).  
Do **not** use it for unauthorized access, credential/session theft, or malicious activity.

---

## Tested Target Environment

- **Application:** Instagram (Android)
- **Version:** `448.0.0.52.84`
- **Build:** `385412061`
- **Platform:** Android
- **Architecture:** `arm64-v8a`

## High-Level Workflow

```text
Android Device (Rooted)
        |
        v
Instagram App
        |
        v
Frida Instrumentation (client + frida-server)
        |
        v
Burp Suite Proxy
        |
        v
HTTPS Traffic Analysis (authorized scope)
```

## Compatibility

| Component | Tested Value | Notes |
|---|---|---|
| App | Instagram `448.0.0.52.84` | Primary validated target |
| Build | `385412061` | Tested build |
| OS Platform | Android | Root access required |
| CPU ABI | `arm64-v8a` | Ensure matching frida-server binary |
| Dynamic Instrumentation | Frida | Client + server required |
| Proxy/Inspection | Burp Suite | Configure CA cert and proxy correctly |

## Requirements

- Rooted Android device (or authorized rooted emulator)
- Frida tools on host machine
- Frida Server on Android device (matching Frida major version)
- Burp Suite (Community or Professional)
- USB or TCP connectivity between host and device

## Installation / Prerequisites

1. Install Frida on your host machine.
2. Push and run `frida-server` on the rooted Android device.
3. Confirm Frida connectivity (device visible from host).
4. Configure device proxy settings to route traffic through Burp Suite.
5. Install and trust Burp CA certificate on the test device for HTTPS interception in your approved lab context.

## Usage (Using Existing Authorized Scripts)

> This repository does **not** provide or modify bypass payload code. Use only your **existing, authorized Frida scripts** from your internal testing toolkit.

Typical execution flow:

1. Start Burp Suite and ensure proxy listener is active (e.g., `127.0.0.1:8080`).
2. Ensure `frida-server` is running on the rooted Android device.
3. Launch Instagram on the device.
4. Attach Frida with your existing script and target package (`com.instagram.android`) in your approved test workflow.
5. Verify HTTPS requests appear in Burp Suite for analysis.

## Burp Suite Configuration Overview

- **Proxy Listener:** Enable listener on host interface/port reachable by device.
- **Device Proxy:** Set Android Wi-Fi proxy to host machine IP and Burp port.
- **CA Certificate:** Install Burp CA on device and trust it for user certs.
- **Scope Control:** Restrict to authorized domains and engagement scope.
- **Validation:** Confirm decrypted HTTPS requests/responses are visible in Burp.

## Troubleshooting

- **Frida device not detected:** Verify USB debugging, `adb devices`, and Frida version compatibility.
- **Script fails to attach:** Confirm package name and app process state.
- **No traffic in Burp:** Recheck device proxy IP/port and host firewall rules.
- **TLS errors persist:** Re-validate certificate trust chain and device trust settings.
- **App/version mismatch:** Re-test against target version/build; behavior may change across releases.

## Tested Versions / Changelog

- **2026-09-26**
  - Documented validated target: Instagram `448.0.0.52.84` (Build `385412061`)
  - Added compatibility matrix, workflow, Burp setup, and troubleshooting notes

## Demo / Screenshots

- `[Placeholder]` Device + Burp proxy settings screenshot
- `[Placeholder]` Frida attachment session screenshot
- `[Placeholder]` Burp HTTP history view screenshot

## Private / Commercial Releases

For private builds, consulting, or commercial testing support:

- **Telegram:** `@your_telegram_handle_here`

---

If you are conducting an engagement, maintain written authorization, clear scope boundaries, and responsible disclosure practices.
