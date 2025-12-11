```markdown
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

**URL:** https://amiunique.org/fp

Steps:

1. Tap **"View my fingerprint"**  
2. Record:
   - **Entropy score**
   - **Number of detectable attributes**

**Expected Safe Result:**

- Not extremely unique  
- Browser fingerprint appears **similar** to other mobile users  
- No catastrophic uniqueness (e.g., 1 in 1M)

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
- Fingerprinting returns *Highly Unique*

Then the device needs further tuning.

### ❌ Fail
If:
- Real IP appears  
- DNS leaks occur  
- Traffic flows when VPN disconnects  

This indicates a misconfiguration.

---

## Follow-Up Actions

Depending on findings:

- Enforce stronger **VPN kill-switch** behavior  
- Harden browser settings  
- Disable WebRTC through browser flags  
- Add a privacy-focused browser  
- Remove apps leaking identifiers  
- Adjust Private DNS  
- Reset Proton VPN settings  
- Re-run DNS and IP leak tests  

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
```
