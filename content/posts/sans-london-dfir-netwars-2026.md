---
title: "SANS London DFIR NetWars 2026"
subtitle: "Back for another NetWars, Forensics this time."
description: "A short look back at SANS London DFIR NetWars 2026: Windows, network and memory forensics, a first-place finish, and putting fvol to use."
date: 2026-11-07
draft: false
featured: true
image: "/images/netwars-london-2026/leaderboard.jpg"
imageAlt: "Final SANS London DFIR NetWars 2026 leaderboard with XSS3cut10n3r in first place"
tags: ["NetWars", "SANS", "ctf", "DFIR", "London"]
---

### The challenges

DFIR NetWars focuses on digital forensics and incident response: working out what happened on a system from the evidence left behind. Challenges covered Windows forensics, network forensics and memory forensics, from user activity and Windows event logs to packet captures, proxy logs and suspicious processes in memory. It was also great to meet Florian, Thomas and Juan, my classmates in London.

After [Core NetWars in Amsterdam](/posts/sans-amsterdam-core-netwars-2026/), London was a chance to spend more time on the forensic side. The challenges involved tracing activity across several machines and connecting evidence from disk, memory and network traffic. In the end I finished in 1st place with 4,576 points, just 28 points ahead of second place.

![Final DFIR NetWars leaderboard with XSS3cut10n3r in first place on 4,576 points](/images/netwars-london-2026/leaderboard.jpg)

### The highlight: memory forensics

Memory forensics was my favorite part of the tournament. The challenges asked us to identify suspicious programs, who ran them and which machines they connected to. Harder questions asked us to find malicious code hidden inside other running programs and identify the malware.

I solved all 12 memory questions using [fvol](https://github.com/code-zm/fvol), a Rust rewrite of Volatility 3 that I helped contribute to. My presets made it quick to work through them.

### The coin

<p style="text-align:center"><img src="/images/netwars-london-2026/coin.jpg" alt="SANS NetWars DFIR Champion coin in its presentation box" width="300"></p>

