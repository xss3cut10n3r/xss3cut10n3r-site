---
title: "Fireplace: Protecting Against Phishing"
subtitle: "Giving users time to stop a takeover without making account recovery impossible"
description: "How Fireplace delays password-only account resets, lets users choose their recovery window, and requires device proof to stop a phishing takeover."
date: 2026-11-16
draft: false
featured: true
tags: ["phishing", "secure messaging", "security design"]
image: "/images/fireplace/fireplace-share.png"
imageAlt: "The Fireplace flame mark and wordmark on a cream background"
---

I want [Fireplace](/posts/fireplace-secure-open-source-chat/) users to sign in with a username and password, without requiring a phone number, email address, or another secret to remember. The difficulty is account recovery: a phisher with stolen credentials can request the same reset as a user who has lost their phone. The design must give the user time to stop a takeover without permanently locking them out when they genuinely need to recover.

A password signs the user into the server; private keys on their devices establish their messaging identity. Knowing the password does not provide those keys. But an immediate reset would let a phisher replace that identity and take over future messaging.

## Recovering with only a password

After losing every device, the user needs a way to create a new messaging identity using their password alone. Requiring an additional recovery secret would defeat that purpose if it was lost too.

When an account already has a device, Fireplace therefore delays the reset. Existing devices show a takeover alert when the app is opened. The user can stop the request from one of those devices by entering their password and choosing a new one. Changing it is part of stopping the takeover because the phisher knows the old password.

<div class="app-screens">
  <figure>
    <img src="/images/secure-messaging/takeover-alert.png" alt="Account takeover alert showing the deadline and a Stop them button." width="780" height="726" loading="lazy">
    <figcaption>The existing device shows the recovery deadline.</figcaption>
  </figure>
  <figure>
    <img src="/images/secure-messaging/stop-takeover.png" alt="Stop them screen requiring the current password and a new password." width="780" height="641" loading="lazy">
    <figcaption>Stopping the request also changes the password.</figcaption>
  </figure>
</div>

If the user still has a trusted device, they can link the new phone by scanning its QR code and skip recovery entirely.

## Choosing the recovery window

Users choose a wait of 1, 3, 7, 14, or 30 days; the default is three. The server timestamps each request and blocks the reset until `request time + configured wait`.

A longer wait gives the user more time to notice and stop an unauthorized reset. The same wait applies if they lose all their devices and need to reset the account themselves.

<div class="app-screens" style="--screen-ratio: 780 / 1147;">
  <figure>
    <img src="/images/secure-messaging/choose-wait.png" alt="Lost-phone wait setting with choices from one to thirty days; three days is selected." width="780" height="1147" loading="lazy">
    <figcaption>One setting determines both recovery delay and time to respond.</figcaption>
  </figure>
  <figure>
    <img src="/images/secure-messaging/recovery-wait.png" alt="New phone waiting for recovery, showing Ready on Wednesday." width="780" height="776" loading="lazy">
    <figcaption>The new phone shows when the reset becomes available.</figcaption>
  </figure>
</div>

If the user misses the deadline, the phisher can complete the reset. Alerts currently appear only when the app is opened; takeover push notifications are not yet implemented.

## Server enforcement

An attacker can bypass the app and call the server directly, so these restrictions must be enforced there. Protected requests carry proof that the caller holds an existing device's private key:

- X25519 establishes a shared secret between the device and server.
- HKDF derives a key specifically for device proofs.
- HMAC-SHA-256 authenticates the account, device, request ID, action, and arguments.

To stop another phone from taking over the account, the user must prove possession of an existing device and re-enter their password. The server checks the token's `auth_time` and rejects authentication older than five minutes. This requires a recent password entry rather than relying on the phone's saved sign-in, preventing a thief with the phone alone from blocking the user's recovery.

The server also prevents someone with only the password from shortening the recovery wait or removing the user's registered devices. Otherwise, a phisher could bypass the delay or remove the phone that would warn the user.

A remaining limitation is that stolen credentials can still be used to delete the account. A cooldown similar to the recovery window will be implemented for account deletion.

If you'd like to contribute to Fireplace, the code is on [GitHub](https://github.com/fireplacechat/mobile).
