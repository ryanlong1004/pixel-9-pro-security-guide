# Pixel 9 Pro Security Hardening Guide

A practical, reproducible checklist for hardening a **Google Pixel 9 Pro** with **Proton VPN** and privacy-focused settings.

---

## Table of Contents

1. [Device Overview](#device-overview)
2. [Lock Screen & Local Security](#lock-screen--local-security)
3. [Google Account Security](#google-account-security)
4. [Proton VPN Configuration](#proton-vpn-configuration)
5. [Android VPN (Always-On) Configuration](#android-vpn-always-on-configuration)
6. [Network & Connectivity Hardening](#network--connectivity-hardening)
7. [App Permissions & Privacy Controls](#app-permissions--privacy-controls)
8. [Browser & Web Hardening](#browser--web-hardening)
9. [Physical Security & Recovery](#physical-security--recovery)
10. [Verification & Testing](#verification--testing)
11. [Future Improvements](#future-improvements)

---

## Device Overview

- **Device:** Google Pixel 9 Pro
- **OS:** Stock Android (Pixel build)
- **Primary account:** Google account with 2FA
- **VPN provider:** Proton VPN (paid or free tier)

This document assumes a clean or mostly-clean device with no root or custom ROM.

---

## Lock Screen & Local Security

Goal: prevent unauthorized physical access and shoulder-surfing.

### Settings

- [x] Set **screen lock** to a **6+ digit PIN**
  - `Settings → Security & privacy → Device lock → Screen lock → PIN`
- [x] **Disable pattern unlock**
- [x] Enable **fingerprint unlock** as a convenience layer on top of PIN
  - `Settings → Security & privacy → Device lock → Fingerprint Unlock`
- [x] Set **Screen lock after sleep** to **Immediately** or **5 seconds**
  - `Settings → Security & privacy → Device lock → Lock after screen timeout`
- [x] Hide sensitive notifications on lock screen
  - `Settings → Notifications → Notifications on lock screen → Hide sensitive content`
- [x] Disable **tap-to-show notification content** if available (optional hardening)

---

## Google Account Security

Goal: harden the identity that unlocks cloud backups, email, and device access.

### Settings

- [x] Enable **2-Step Verification (2FA)** on primary Google account
  - Use **security keys** or **Google Prompt** over SMS-based codes.
- [x] Review and clean up **recovery methods**
  - Remove old phone numbers and secondary emails no longer in use.
- [x] Review **Security Activity** and **Devices**
  - Revoke access for devices or sessions you don’t recognize.
- [x] Turn on **Advanced Protection Program** (optional, high-security users)

---

## Proton VPN Configuration

Goal: enforce encrypted traffic, DNS privacy, and minimal leaks.

### Proton VPN App Settings

- [x] **Sign in** with Proton account (e.g., `ryan@rlong.dev`)
- [x] Enable **NetShield**
  - `Proton VPN → Settings → NetShield → Malware + Trackers`
- [x] Disable **Split tunneling**
  - `Proton VPN → Settings → Split tunneling → Off`
- [x] Enable **Kill switch**
  - `Proton VPN → Settings → Kill switch → On`
- [x] Set **Default connection** to `Fastest country` or a preferred region
  - `Proton VPN → Settings → Default connection → Fastest country`
- [x] Set **Protocol** to **WireGuard**
  - `Proton VPN → Settings → Protocol → WireGuard`
  - (Avoid “Smart (auto)” if you want deterministic behavior.)

---

## Android VPN (Always-On) Configuration

Goal: prevent _any_ traffic from leaving the device outside of the VPN tunnel.

### System Settings

- [x] Open `Settings → Network & internet → VPN`
- [x] Tap **Proton VPN** (gear icon)
- [x] Enable:
  - [x] **Always-on VPN**
  - [x] **Block connections without VPN**

Result:  
If Proton VPN disconnects, network traffic is blocked at the OS level (system kill switch).

---

## Network & Connectivity Hardening

Goal: reduce risk from insecure cell networks, Wi-Fi spoofing, and passive tracking.

### Mobile Network

- [x] Disable **2G** (prevents downgrade attacks)
  - `Settings → Network & internet → SIMs → Allow 2G → Off`
- [x] Prefer **4G/5G** automatic (default)

### Wi-Fi

- [x] Disable auto-connect to **open networks**
  - `Settings → Network & internet → Internet → Network preferences`
- [x] Enable **MAC address randomization**
  - `Wi-Fi network → Privacy → Use randomized MAC`

### Bluetooth & Scanning

- [x] Turn **Bluetooth off** when not in use
- [x] Disable **Bluetooth scanning** (and optionally Wi-Fi scanning)
  - `Settings → Location → Location services → Wi-Fi scanning / Bluetooth scanning → Off`

### Private DNS (Optional)

- [x] Configure **Private DNS** (optional extra DNS privacy)
  - `Settings → Network & internet → Private DNS → Private DNS provider hostname`
  - Example: `dns.proton.me` or `dns.quad9.net`

> Note: When Proton VPN is active, its own DNS is used; Private DNS is mostly a fallback for non-VPN scenarios.

---

## App Permissions & Privacy Controls

Goal: minimize unnecessary access to sensors, location, data, and storage.

### Permission Manager Audit

- [x] Open `Settings → Privacy → Permission manager`
- For each category, ensure:
  - [x] **Location**
    - Only mapping / weather apps get `Allow only while using`.
    - Disable for apps that do not truly require it.
  - [x] **Camera** and **Microphone**
    - Restrict to messaging / camera apps.
    - Deny for random utilities or games.
  - [x] **Contacts**, **Call logs**, **SMS**
    - Only phone, messages, and essential communication apps should have this.
  - [x] **Files and media**
    - Prefer **scoped storage** access instead of full file system access.

### Global Toggles

- [x] Use quick settings **Camera** and **Mic** toggles when not actively needed.
- [x] Uninstall apps unused for ~60 days to remove stale permission sets.

---

## Browser & Web Hardening

Goal: reduce web tracking, fingerprinting, and malicious script exposure.

These settings can be applied to Chrome, Firefox, Brave, or another main browser.

### General

- [x] Enable **HTTPS-only / Always secure connections** (where supported)
- [x] Block **third-party cookies** or use **“Block cross-site tracking”**
- [x] Turn on enhanced safe browsing / phishing protection (browser-specific)
- [x] Use a content blocker (e.g., uBlock Origin where extensions are supported)

### Optional

- [ ] Use a **privacy-focused browser** (e.g., Firefox with hardened config, Brave, or a sandboxed browser)
- [ ] Disable **WebRTC** leaks (if available) in browser settings or via extension

---

## Physical Security & Recovery

Goal: protect data even if the device is lost, stolen, or confiscated.

### Device

- [x] Confirm full-disk encryption (enabled by default on Pixel)
- [x] Enable **Find My Device**
  - `Settings → Security & privacy → Find My Device`
- [x] Enable **Lockdown Mode** for high-risk situations
  - Press and hold power button → **Lockdown** (disables biometrics until PIN is entered)
- [x] Optionally: enable **Auto factory reset** after multiple failed unlock attempts (if available in region/build.)

### Recovery & Backup

- [x] Keep **recovery codes** and **hardware security key** stored offline in a safe location.
- [x] Use **encrypted cloud backups** (Google One, Proton, etc.) for essential data.
- [x] Avoid unencrypted local exports of account data.

---

## Verification & Testing

Goal: confirm that VPN, DNS, and privacy controls behave as expected.

### VPN / IP / DNS Leak Tests

With Proton VPN **connected** and **always-on** enabled:

- [x] Visit an IP-check service (e.g., `https://ipleak.net` or similar)
  - Confirm:
    - IP address = Proton VPN exit node
    - Location = VPN country, not real location
- [x] Confirm **DNS servers** belong to Proton or privacy-respecting DNS provider.
- [x] Temporarily disconnect Proton VPN:
  - Verify that **no traffic flows** (because “Block connections without VPN” is enabled).
  - Reconnect and confirm normal operation returns.

### App Behavior

- [x] Open apps that previously used location/camera/mic:
  - Confirm they prompt for permission.
  - Deny if not strictly necessary.

---

## Future Improvements

Ideas for further hardening beyond this baseline:

- [ ] Use a **separate Google account** for the device with minimal data tied to identity.
- [ ] Move critical authentication to **hardware security keys** only (where supported).
- [ ] Use **sandboxed / work profile** for untrusted apps (via Shelter or native work profile).
- [ ] Consider a **hardened browser profile** for sensitive tasks (banking, personal accounts).
- [ ] Periodically review:
  - Installed apps
  - VPN configuration
  - Google account security alerts
  - Permissions and notification settings

---

_Last updated: {{UPDATE_DATE_HERE}}_
