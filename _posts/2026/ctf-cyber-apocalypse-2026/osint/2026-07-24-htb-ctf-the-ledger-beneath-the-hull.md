---
title: The Ledger Beneath the Hull
description: Can you trace the hull beneath the painted name?
date: 2026-07-24 10:34:00
categories: [CTF, HTB - Cyber Apocalypse 2026]
tags: [osint]
pin: false
image:
  path: /assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/court-console.png 
  alt: Analyst Briefing
---

## Challenge Summary
**Challenge Scenario**

> Lord Damas Marrowcairn does not command fleets — he owns them through a labyrinth of paper. A single cargo vessel, the ASHEN MERCY, sits at the center of a web spun across five companies: a nominee-owned shell that holds the hull, a management firm that runs the machinery, a coordination house that directs the voyages, and a commodities trader that fills the holds. Each layer is clean. Each layer is someone else's name. But every thread, if pulled hard enough, runs back to the same hand. The Outer Isles P&I Club has opened its registry for inspection — registry files, charter fixtures, company ledgers, and P&I entries are all available to those who know where to look. Reconstruct the full ownership chain. Prove that the hand beneath the ink belongs to Marrowcairn.
>
>Use the Oath Submission form to confirm your findings, then assemble the flag from the verified answers.
>
> Flag Format: HTB{ITEM_WAS_SHARED_AMONG_QUANTITY_TARGETS}
> 
> Flag Example: HTB{THE_WATER_WAS_RATIONED_AMONG_SIX_VILLAGES}

**Challenge Info**

|:------:|:------:|
| Category | OSINT
| Points | 875 |
| Difficulty | Easy
| Creator | Joey
| Release Date | 2026/07/24

## Solution

### End Goal

Our challenge scenario states that our end goal is to reconstruct the full ownership chain.

> Reconstruct the full ownership chain. Prove that the hand beneath the ink belongs to Marrowcairn.

### The Court-Eaves Console

As with any OSINT challenge, we want to understand two things. The tools that are available and the questions that need to be answered.

**Tools Provided**

![Tools Provided](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/tools-provided.png)

1. Briefing
2. Tideglass
3. Registry
4. P&I DIR
5. Charter
6. Companies
7. Evidence
8. Threadboard
9. Submit

**Findings Submission**

![Submission](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/submission.png)
![Submission 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/submission-2.png)

As we go through the investigation, keep these questions in the back of your mind.

1. Who is the vessel's registered owner?
2. Who is the vessel's ISM manager?
3. Which company is listed as the commercial operator?
4. Which company is the time charterer?
5. Which parent company ultimately control both the operator and the charterer?

### Case Briefing

In our case briefing we're given some data points to start connecting the dots. 

![Case Briefing](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/briefing.png)

From the briefing, the following data points stick out:

|:----------------:|:------------:|
| Vessel IMO       | 9724418    
| P&I Entry        | PI-VAL-88291
| Release Approver | Eastreach Maritime Coordination PLC

### Maritime Registry

The `Registry` requires an `IMO number`, `vessel name`, or `registry name`. Right now, we only have the Vessel IMO number given in the briefing - `9724418`.

![Registry](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/registry.png)
![Registry 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/registry-2.png)

**Information Obtained from the Maritime Registry**

|:---------------------:|:------------:|
| Vessel Name           | Ashen Mercy
| MMSI                  | 636019772
| Radio Call Sign       | D5MR8
| Flag State / Register | Outer Isles Register
| Vessel Type           | General Cargo
| Year Built            | 2014
| Registry File         | OIR-9724418-2026
| Registered Owner      | Thirteenth Tide Shipping Ltd
| ISM Manager           | Morrow Fleet Management SA
| Technical Manager     | Morrow Fleet Management SA


Then maritime registry provided more specific information we can use to track this vessel and the companies operating it. Looking back at our `Evidence Satchel`, we can see our finding is already populated.

![Maritime Registry Evidence](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/evidence-satchel.png)

We can also start to fill out the `Threadboard`:

![Maritime Registry Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/threadboard.png)

The note at the bottom of the Maritime Registry states that this record does indicate commercial operation or charter arrangements and to consult the `P&I Directory` for operational details.

### P&I Directory

Pivoting over to the `P&I Directory`. Here we can filter by `entity number`, `IMO number`, or `vessel name`. Let's use the entity number (`P&I Entry`) provided in the briefing - `PI-VAL-88291`. 

![PI-VAL-88291 Directory](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/pi-directory.png)
![PI-VAL-88291 Directory 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/pi-directory-2.png)
![PI-VAL-88291 Directory 3](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/pi-directory-3.png)

**Information Obtained from the P&I Directory**

|:---------------------------:|:------------:|
| Member / Registered Owner   | Thirteenth Tide Shipping Ltd
| Co-Assured                  | Morrow Fleet Management SA
| Commercial Operator         | Eastreach Maritime Coordination PLC
| Time Charterer              | Gilded Knife Commodities Ltd
| Entry Period                | 2026-01-01 to 2026-12-31
| Trading Area                | Valyssar coast, Stormcoast, Northern Crown Road ports
| Certificate Status          | Active

The `P&I Directory` has provided more information about the companies operating the vessel `Ashen Mercy`, including the commercial operator and time charterer. Again, our `Evidence Satchel` has been populated with our new findings.

![Maritime Registry Evidence](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/evidence-satchel-2.png)

We can also add these additions to our `Threadboard`:

![P&I Directory Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/threadboard-2.png)

The analyst note at the bottom of the P&I Directory states the commercial operator and time charterer listed in this entry differ from the registered owner (`Thirteenth Tide Shipping Ltd`). It goes on to say "this is standard for time-chartered vessels operating under third party commercial management" and to investigate the corporate relationships between these companies.

### Companies Register

The `Companies Register` allows us to look up information about all the companies we've discovered during this investigation.

#### Thirteenth Tide Shipping Ltd

![Thirteenth Tide Shipping Ltd Company](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-thirteenth.png)

`Thirteenth Tide Shipping Ltd` is a single-vessel company with the following identifiers 

|:---------------- :|:------------:|
| Company Number    | OI-771204
| Registered Office | 80 Seaward House, Port Cinder
| Directors         | `Nominee Maritime Directors Ltd`
| Shareholders      | `Morrow Nominee Holdings SA`

#### Morrow Fleet Management SA

![Morrow Fleet Management SA Company](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-morrow-fleet.png)
![Morrow Fleet Management SA Company 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-morrow-fleet-2.png)

`Morrow Fleet Management SA` has the following identifiers 

|:---------------- :|:------------:|
| Company Number    | ER-441902
| Registered Office | 14 Anchor Court, Eryndal
| Directors         | `Deren Morrow` and `Sela Quill`
| Shareholders      | `Morrow Marine Group SA - 100%`


#### Eastreach Maritime Coordination PLC

![Eastreach Maritime Coordination PLC](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-eastreach.png)
![Eastreach Maritime Coordination PLC 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-eastreach-2.png)

`Eastreach Maritime Coordination PLC` has the following identifiers 

|:---------------- :|:------------:|
| Company Number    | ER-220014
| Registered Office | 3 Saltmarsh Lane, Eryndal
| Directors         | `Damas Marrowcairn`, `Leven Orr`, and `Sera Dain`
| Shareholders      | `Marrowcairn Strategic Holdings PLC - 100%`
| Parent Company    | `Marrowcairn Strategic Holdings PLC`

#### Gilded Knife Commodities Ltd

![Gilded Knife Commodities Ltd](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-gilded.png)
![Gilded Knife Commodities Ltd 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-gilded-2.png)

`Gilded Knife Commodities Ltd` has the following identifiers 

|:---------------- :|:------------:|
| Company Number    | ER-220881
| Registered Office | 3 Saltmarsh Lane, Eryndal
| Directors         | `Leven Orr`
| Shareholders      | `Marrowcairn Strategic Holdings PLC - 100%`
| Parent Company    | `Marrowcairn Strategic Holdings PLC`

Both `Eastreach Maritime Coordination PLC` and `Gilded Knife Commodities Ltd` have the same parent company `Marrowcairn Strategic Holdings PLC` and have the same registered office of `3 Saltmarsh Lane, Eryndal`. Both companies also list `Leven Orr` as one of their directors.

#### Marrowcairn Strategic Holdings PLC

![Marrowcairn Strategic Holdings PLC](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-marrowcairn.png)
![Marrowcairn Strategic Holdings PLC 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/company-marrowcairn-2.png)

`Marrowcairn Strategic Holdings PLC` has the following identifiers 

|:---------------- :|:------------:|
| Company Number    | ER-009901
| Registered Office | 1 Crown Passage, Eryndal
| Directors         | `Damas Marrowcairn`
| Shareholders      | `Damas Marrowcairn - 72%` and `Unnamed institutional investors - 28%`

Our `Evidence Satchel` has been populated with our findings.

![Companies Register Evidence](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/evidence-satchel-3.png)
![Companies Register Evidence](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/evidence-satchel-4.png)

Let's also update our `Threadboard` with our new findings: 

![Companies Register Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/threadboard-complete.png)

### Findings Submission

In `Submit`, we can use all the evidence gathered to submit the answers to the questions.

![Submission Quote](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/submission-quote.png)
![Questions Answered 1](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/submission-complete-1.png)
![Questions Answered 2](/assets/img/2026-posts/cyber-apocalypse/the-ledger-beneath-the-hull/submission-complete-2.png)

> The hull belongs to a paper company. The machinery answers to a manager. The voyages answer to Eastreach. Beneath every layer sits the same hand, clean because the ink has been divided among five names.

Our task was to "prove that the hand beneath the ink belongs to Marrowcairn." Throughout this challenge, we were able to use the `Maritime Registry`, `P&I Directory`, and the `Companies Register` to prove `Marrowcairn Strategic Holdings PLC` owns `Eastreach Maritime Coordination PLC` and `Gilded Knife Commodities Ltd`. Both of these companies operate and charter the `Ashen Mercy` vessel respectively.

The challenge scenario specified the flag format is: `HTB{ITEM_WAS_SHARED_AMONG_QUANTITY_TARGETS}`. 
* The item that was shared was `the ink`.
* The ink was `divided` among `five names` (companies).

Putting the clues together in the quote, leaves us with the following flag: `HTB{THE_INK_WAS_DIVIDED_AMONG_FIVE_NAMES}`.

