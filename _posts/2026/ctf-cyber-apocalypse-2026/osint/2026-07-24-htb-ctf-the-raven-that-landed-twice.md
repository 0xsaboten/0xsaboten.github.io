---
title: The Raven That Landed Twice
description: Can you cross-reference the Aircraft Registry, Movement Ledger, Courier Mail, and Skyglass Browser to prove the raven that landed twice was a single aircraft, and expose the operator hiding behind a leasing company's name?
date: 2026-07-24 10:15:00
categories: [CTF, HTB - Cyber Apocalypse 2026]
tags: [osint]
pin: false
image:
  path: /assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/court-console.png 
  alt: Court-Eaves Console
---

## Challenge Summary

**Challenge Scenario**

> Suncourt keeps its messengers arriving without looking like messengers — a private aircraft leaves records no one thinks to compare. On the night before three Registry witnesses changed their testimony, a dark executive aircraft slipped into Crownspire Executive Field under a sealed passenger list. But the dispatch slip and fuel sheet were not sealed: a gate clerk copied the aircraft's Mode-S hex 43E91C and its callsign VLR602. Miren Vale, the Aerial Witness Desk's analyst, suspects the same flight was recorded twice — once in the sky under its callsign, once on the ground under its registration — to hide who truly operated it. The passenger manifest is sealed under Court Courtesy Order CCO-2026-0441, but the technical identifiers remain open. Can you cross-reference the Aircraft Registry, Movement Ledger, Courier Mail, and Skyglass Browser to prove the raven that landed twice was a single aircraft, and expose the operator hiding behind a leasing company's name? 
>
> Use the Oath Submission app to confirm your findings, then submit the flag in the following format:
> 
> HTB{REGISTRATION_FROM_DEPARTURE_AERODROME_TO_PARKING_STAND}
>
> Example: HTB{G-NTWK_FROM_BRINDLE_MOOR_TO_7C}

**Challenge Info**

|:------:|:------:|
| Category | OSINT
| Points | 850 |
| Difficulty | Very Easy
| Creator | Joey
| Release Date | 2026/07/24


## Solution

### End Goal

Our challenge scenario states that our end goal is to find the hidden operator.

> Can you cross-reference the Aircraft Registry, Movement Ledger, Courier Mail, and Skyglass Browser to prove the raven that landed twice was a single aircraft, and expose the operator hiding behind a leasing company's name?

### Court-Eaves Console

As with any OSINT challenge, we want to understand two things. The tools that are available and the questions that need to be answered.

**Tools Provided**

![Tools Provided](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/tools-provided.png)

1. Case Briefing
2. Skyglass Browser
3. Aircraft Registry
4. Movement Ledger
5. Courier Mail
6. Evidence Satchel 
7. Oath Submission

**Evidence Required**

![Evidence](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/evidence-satchel.png)

We need to collect the `registration`, `operator`, `departure`, and `stand`. If we take a look at `Oath Submission`, we can see the evidence lines up with the questions that need to be answered.

![Submission](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/oath-submission.png)

As we go through the investigation, keep these questions in the back of your mind.

1. What is the aircraft registration?
2. Which company operates the aircraft?
3. From which aerodrome did the aircraft depart?
4. Which stand did it use at Crownspire Executive Field

### Case Briefing

In our case briefing we're given some data points to start connecting the dots. 

![Case Briefing](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/case-briefing.png)

From the briefing, two data point stick out:

|:---------------:|:------------:|
| MODE-S HEX      | 43E91C    
| FLIGHT CALLSIGN | VLR602

### Aircraft Registry 

The `Aircraft Registry` requires a `Mode-S hex`, `registration`, `operator` or `serial` number. Right now, we only have the Mode-S Hex number given in the briefing - `43E91C`.

![Aircraft Registry](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/aircraft-registry.png)

![Aircraft Registry 2](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/aircraft-registry-2.png)

**Information Obtained from the Aircraft Registry**

|:---------------:|:------------:|
| Registration    | 2-RUNE
| ICAO Type Code  | F2TH
| Aircraft Type   | Dassault Falcon 2000EX
| Serial Number   | 182
| Registration Date | 2024-02-19
| Last Update     | 2026-07-10 
| Registered Owner | Black Keep Leasing SPC
| Operator        | Vaultrune Air Services Ltd
| Base Aerodrome  | Blackwell Aerodrome
| Known Callsigns | `VLR602`, `VLR600`, `VRUN2`

The aircraft registry provided more specific information we can use to track this aircraft. At the bottom of this page, let's `tag registration` and `tag operator`.

![Aircraft Registry Evidence](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/aircraft-registry-evidence.png)

Looking back at our `Evidence Satchel`, we can see we've already found two key findings:

![Aircraft Registry Evidence](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/evidence-satchel-2.png)

### Movement Ledger

Next, let's pivot over to the `Movement Ledger`. Here we can filter by `callsign`, `registration`, `Mode-S`, `stand`, `aerodrome`. We know this aircraft uses three callsigns, let's use the newly discovered registration - `2-RUNE`. Only one result is returned:

![2-RUNE Movement](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/movement-ledger.png)
![2-RUNE Movement 2](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/movement-ledger-2.png)
![2-RUNE Movement 3](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/movement-ledger-3.png)

**Information Obtained from the Movement Ledger**

|:---------------:|:------------:|
| Parking Stand   | 4B
| Departure Aerodrome | Suncourt Field (SCF)
| Arrival Aerodrome | Crownspire Executive Field (CSE)
| Off-Block       | 2026-07-17 23:58 UTC
| Landing         | 2026-07-18 00:47 UTC
| On-Block        | 2026-07-18 00:51 UTC
| Ground Handler  | Crownspire Crown Aviation Services


The aircraft departed from `Suncourt Field (SCF)` at `2026-07-17 23:58 UTC` and arrived at `Crownspire Executive Field (CSE)` at `2026-07-18 00:47 UTC`. The aircraft arrived at the parking stand `4B` at `2026-07-18 00:51 UTC`. At the bottom of this page, let's `tag stand` and `tag departure`.

Looking back at our `Evidence Satchel`, we can see we've uncovered all the key findings:

![Movement Ledger Evidence](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/evidence-satchel-3.png)
![Movement Ledger Evidence 2](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/evidence-satchel-4.png)


### Oath Submission

On `Oath Submission`, we can use all the evidence gathered to submit the answers to the questions.

![Questions Answered](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/oath-submission-complete.png)
![Evidence Quote](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/oath-case-complete.png)

> Miren draws one line between two records. The raven did not land twice. It was described twice - once by the sky, once by the ground. Suncourt has begun moving whispers faster than horses.

We're also given a review of all the evidence we collected. Throughout this challenge, we were able to cross-reference the `Aircraft Registry` and `Movement Ledger` to prove the raven that landed twice was a single aircraft using the Mode-S Hex `43E91C`. We also determined the aircraft is operated by `Vaultrune Air Services Ltd` and not `Black Keep Leasing SPC` as listed on the registration.

![Evidence Review](/assets/img/2026-posts/cyber-apocalypse/the-raven-that-landed-twice/evidence-review.png)

The challenge scenario specified the flag format is: `HTB{REGISTRATION_FROM_DEPARTURE_AERODROME_TO_PARKING_STAND}`. 
* The registration was `2-RUNE`.
* The departure aerodrome was `Suncourt Field`. 
* The parking stand was `4B`. 

Putting the clues together leaves us with the following flag: `HTB{2-RUNE_FROM_SUNCOURT_FIELD_TO_4B}`.