# Pixel 9 Pro Privacy Test Guide

A structured, step-by-step privacy audit designed to validate the security posture of a **Google Pixel 9 Pro** using **Proton VPN**.  
This document can be committed directly to a GitHub repository.

---

## Table of Contents

1. [Overview](#overview)
2. [Test Preparation](#test-preparation)
3. [Step 1 — IP Leak Test](#step-1--ip-leak-test)
4. [Step 2 — DNS Leak Test](#step-2--dns-leak-test)
5. [Step 3 — WebRTC Leak Test](#step-3--webrtc-leak-test)
6. [Step 4 — Fingerprinting Test](#step-4--fingerprinting-test)
7. [Step 5 — Tracker Exposure Test](#step-5--tracker-exposure-test)
8. [How to Interpret Results](#how-to-interpret-results)
9. [Follow-Up Actions](#follow-up-actions)
10. [Appendix — Expected Safe Outputs](#appendix--expected-safe-outputs)

---

## Overview

This guide verifies that your hardened Pixel 9 Pro setup:

- Does **not** leak your real IP
- Does **not** leak DNS requests
- Does **not** expose WebRTC public IP
- Is not uniquely trackable
- Properly blocks trackers and fingerprint scripts

The goal is to ensure that **Proton VPN + Android's Always-On VPN Mode** are functioning as intended.

---

## Test Preparation

Before starting, ensure the following:

- Proton VPN is **connected**
- Protocol is set to **WireGuard**
- Android settings have:
  - **Always-on VPN** enabled
  - **Block connections without VPN** enabled
- Split tunneling is **disabled**

Optional but recommended:

- Clear browser cache
- Close all apps except your browser

---

## Step 1 — IP Leak Test

**URL:** https://ipleak.net

Record the following:

- Displayed **public IP**
- Displayed **country**
- Number of IP addresses detected
- Any IPv6 address shown

**Expected Safe Result:**

- Only **one IP**
- Belongs to **Proton VPN**
- Location = VPN region, not your real location
- No IPv6 unless Proton provides one

---

## Step 2 — DNS Leak Test

Scroll to the **DNS Addresses** section on:

https://ipleak.net

Record:

- Number of DNS servers shown
- Country of each DNS server
- Whether any DNS servers belong to:
  - Your ISP
  - Google (8.8.8.8)
  - Cloudflare (1.1.1.1)
  - AT&T / Verizon
  - Any local hostname

**Expected Safe Result:**

- All DNS servers belong to **Proton** or anonymized servers
- Zero DNS entries tied to your ISP
- No domestic DNS servers revealing your region

---

## Step 3 — WebRTC Leak Test

**URL:** https://browserleaks.com/webrtc

Record:

- **Local IP addresses** (usually 192.168.x.x or 10.x.x.x)
- **Public IP address** (if shown)

**Expected Safe Result:**

- Local IP = private range only
- Public IP = VPN IP OR empty
- No exposure of your real IPv4 or IPv6

---

## Step 4 — Fingerprinting Test

**URL:** https://amiunique.org/

Steps:

1. Tap **"View my browser fingerprint"**
2. Record:
   - **Uniqueness score**
   - **Number of detectable attributes**

**Expected Safe Result:**

- Not extremely unique
- Browser fingerprint appears **similar** to other mobile users
- No catastrophic uniqueness (e.g., 1 in 1M)

**⚠ Note:** Mobile devices often show as unique due to hardware/software combinations. This is a **known limitation** - VPNs mask IP but don't prevent browser fingerprinting.

---

## Step 5 — Tracker Exposure Test

**URL:** https://coveryourtracks.eff.org/

Run the test and record:

- Tracker blocking score
- Fingerprinting result:
  - `Unique`
  - `Not unique`
  - `Partially protected`

**Expected Safe Result:**

- Tracker blocking = **Strong**
- Fingerprinting = **Not unique** or **Randomized**

---

## How to Interpret Results

### ✔ Pass

If your results match expected safe ranges, your Pixel 9 Pro is:

- Leak-proof
- Fingerprint-hardened
- VPN-enforced
- Tracker-resistant

### ⚠ Partial Pass

If:

- You see **multiple IPs**
- DNS shows your ISP
- WebRTC shows a public IP
- Fingerprinting returns _Highly Unique_

Then the device needs further tuning.

### ❌ Fail

If:

- Real IP appears
- DNS leaks occur
- Traffic flows when VPN disconnects

This indicates a misconfiguration.

---

## Follow-Up Actions

### If You Have IP/DNS Leaks

- Enforce stronger **VPN kill-switch** behavior
- Verify Always-on VPN is enabled: `Settings → Network & Internet → VPN → Proton VPN → Always-on VPN`
- Enable **Block connections without VPN**
- Disable split tunneling in Proton VPN app
- Adjust Private DNS to `off` or `dns.quad9.net`
- Reset Proton VPN settings and reconnect
- Re-run DNS and IP leak tests

### If You Have a Unique Fingerprint

Browser fingerprinting happens at the client level and **cannot be prevented by VPN alone**. To reduce uniqueness:

#### 1. **Switch to a Privacy-Focused Browser**
   - **Brave Browser** - Built-in fingerprint randomization
   - **Firefox Focus** - Minimal tracking, auto-clears history
   - **Bromite** - Chromium-based with privacy patches
   - **Tor Browser** (extreme) - Maximum anonymity

#### 2. **Harden Chrome/Chromium**
   - Disable JavaScript for sensitive sites:
     - `Settings → Site settings → JavaScript → Block`
   - Disable third-party cookies:
     - `Settings → Site settings → Cookies → Block third-party cookies`
   - Clear cookies/cache regularly

#### 3. **Install Content Blockers (Firefox)**
   - **uBlock Origin** - Blocks trackers and ads
   - **Privacy Badger** - Learns and blocks invisible trackers
   - **CanvasBlocker** - Prevents canvas fingerprinting

#### 4. **Disable Fingerprinting Vectors**
   - Turn off location services for browsers:
     - `Settings → Apps → [Browser] → Permissions → Location → Don't allow`
   - Disable motion sensors (if supported):
     - `Settings → Privacy → Motion sensors → Off`
   - Use standard time zone (UTC) in browser if possible

#### 5. **Standardize Your Configuration**
   - Use common screen resolutions (avoid rare aspect ratios)
   - Disable custom fonts
   - Avoid browser extensions (they can be fingerprinted)
   - Use default language settings (English US is most common)

#### 6. **Advanced: Browser Flags (Chrome/Brave)**
   Navigate to `chrome://flags` or `brave://flags`:
   - Disable **WebGL**: Search "WebGL" and disable
   - Disable **WebRTC**: Install extension like "WebRTC Leak Shield"
   - Enable **Do Not Track**: `Settings → Privacy and security → Send a 'Do Not Track' request`

#### 7. **Accept the Trade-off**
   - **Reality check**: Perfect anonymity requires Tor Browser with strict settings
   - Mobile fingerprinting is **very difficult** to eliminate completely
   - Focus on IP/DNS protection (which VPN handles) and tracker blocking
   - Unique fingerprint ≠ immediate threat (depends on your threat model)

### General Maintenance

- Remove apps leaking identifiers (check app permissions)
- Review Location History: `Google Account → Data & Privacy → Location History`
- Disable ad personalization: `Google Account → Data & Privacy → Ad settings`
- Run these tests monthly to detect configuration drift

---

## Appendix — Expected Safe Outputs

### Example “Good” IP Leak Test

```

IP Address: 185.159.xxx.xxx
Country: Switzerland
IPv6: None

```

### Example “Good” DNS Leak Test

```

DNS Server 1: Proton AG (CH)
DNS Server 2: Proton AG (IS)
Total servers: 2

```

### Example “Good” WebRTC Test

```

Local IPs: 192.168.1.25
Public IP: 185.159.xxx.xxx (VPN)
Real IP: Not exposed

```

### Example “Good” Fingerprint Result

```

Entropy Score: Medium
Attributes: 28
Fingerprint: Not unique among tested devices

```

### Example “Good” Tracker Exposure Result

```

Tracker Blocking: Strong
Fingerprinting: Not unique

```

---

_Last updated: {{UPDATE_DATE_HERE}}_
