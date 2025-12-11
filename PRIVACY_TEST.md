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
