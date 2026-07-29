---
title: The Helpers With Clean Hands
description: Trace the structure end to end and file the findings.
date: 2026-07-24 10:41:00
categories: [CTF, HTB - Cyber Apocalypse 2026]
tags: [osint]
pin: false
image:
  path: /assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/analyst-briefing.png 
  alt: Analyst Briefing
---

## Challenge Summary
**Challenge Scenario**

> The war of succession has ended on paper, but the Northern Crown Road still bleeds quietly. Villages that once resisted the Crown's claim now receive "mercy" from the Mercy Lantern Relief Trust — a charity delivering burial candles, lamp oil, and grief counselling to communities broken by the fighting. Behind the trust stands Ash & Wick Provisioners Ltd, a wholesale supplier of ceremonial goods whose invoices are clean, whose warehouse is spotless, and whose registered office is a letterbox on Cinder Lane. The charity's trustees believe they serve the grieving. The supplier's director, Nera Sorn, knows otherwise. Above Sorn sits Quiet Mercy Holdings Ltd, a non-trading shell wholly owned by a fiduciary foundation with restricted disclosure — a structure designed so that the same handful of names can sign both the purchase orders and the relief manifests without any single office ever being answerable. The next delivery is scheduled for Harrowgate. The receiving clerk is Sister Merrow. Somewhere in the tender records, the company filings, and the couriered dispatch notes, the chain of control runs from a charity that teaches mercy to a holding company that appoints the hands that make one possible. An intelligence desk has been set up with access to the Companies Register, the Tender Hall, an archive mirror, and the courier's intercepted mail. Trace the structure end to end and file the findings.
>
>Use the Oath Submission form to confirm your findings, then assemble the flag from the verified answers.
>
>Flag Format: HTB{SUPPLIER_FEEDS_TOWN}
>
>Flag Example: HTB{COMPANY_NAME_FEEDS_DELIVERY_LOCATION}


**Challenge Info**

|:------:|:------:|
| Category | OSINT
| Points | 900 |
| Difficulty | Easy
| Creator | Joey
| Release Date | 2026/07/24

## Solution

### End Goal

> Find the company behind VR-118204. Do not assume the charity and the supplier are the same legal entity.

### The Court-Eaves Console

As with any OSINT challenge, we want to understand two things. The tools that are available and the questions that need to be answered.

**Tools Provided**

![Tools Provided](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/tools-provided.png)

1. Briefing
2. Register
3. Tenders
4. Archive
5. Browser
6. Mail
7. Satchel
8. Threads
9. Submit

**Findings Submission**

![Submission](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/submission.png)

As we go through the investigation, keep these questions in the back of your mind.

1. Which commercial company supplies the Mercy Lantern Relief Trust?
2. Which company owns 100% of that supplier?
3. Which directory appears across both the supplier and its holding company?
4. Which town is scheduled to receive the next delivery under tender ML-22-771?
5. Who is listed as the Lot 3 receiver?

### Analyst Briefing

In our analyst briefing we're given some data points to start connecting the dots. 

![Analyst Briefing](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/analyst-briefing-zoom.png)

From the briefing, the following data points stick out:

|:---------:|:------------:|
| REFERENCE	| ML-22-771
| PURCHASER | Mercy Lantern Relief Trust
| SUPPLIER REGISTRATION | VR-118204
| DELIVERY REGION | Northern Crown Road

### Register - Mercy Lantern Relief Trust

The `Register` requires a `name`, `number`, `directory`, `shareholder`, `address`, or a `former name`. Let's start with `Mercy Lantern Relief Trust`.

![Register](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/mlrt-register.png)

**Information Obtained from the Register**

|:---------------------:|:------------:|
| Charity Number        | CH-44019
| Registered Office     | 7 Quiet Bell Court, Crownspire
| Trustees              | `Sister Merrow`, `Caldus Vey`, and `Orren Hald`
| Administrative Contact | `Ilyra Venn`

The register reveals the charity number and we know more about the appointed people within the charity.

Make sure to save this evidence to the satchel with the `Save to Satchel` button. After that, we can start to fill out the `Analyst Threadboard`:

![Register Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/threadboard.png)

### Tenders

Pivoting over to the `Tenders`. Here we can filter by `reference`, `purchaser`, or `town`. Let's use the reference provided in the briefing - `ML-22-771`. 

![Tender](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/mlrt-tender.png)
![Tender 2](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/mlrt-tender-2.png)

**Information Obtained from the Tender**

The most important piece of information we've uncovered is the supplier is `Ash & Wick Provisioners Ltd` and the company number is `VR-118204`.

We also obtained the delivery schedule:

| Lot | Town    | Date       | Status    | Receiver   | REF
|:---:|:-------:|:----------:|:---------:|:----------:|:----------:|
| 1 | Riverwake | 2026-07-12 | Completed | Orren Hald | AWP-RW-4406
| 2 | Stoneford | 2026-07-16 | Completed | Caldus Vey | AWP-SF-4407
| 3 | Harrowgate | 2026-07-21 | Scheduled | Sister Merrow | AWP-HG-4408

Again, let's save to satchel and update the `Analyst Threadboard`:

![Tender Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/threadboard-2.png)

### Register - Ash & Wick Provisioners

Let's head back to the `Register`, but this time looking up `Ash & Wick Provisioners Ltd`.

![Register](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/aw-register.png)

**Information Obtained from the Register**

|:---------------------:|:------------:|
| Registered Office     | 14 Cinder Lane, Eastreach
| Former Trading Name   | Ashwick Municipal Supply
| Directory             | `Nera Sorn`
| Company Secretary     | Vey & Marr Corporate Services
| Shareholder           | Quiet Mercy Holdings Ltd - 100% 

We know have a better under standings of the inner workings at `Mercy Lantern Relief Trust` and  `Ash & Wick Provisioners Ltd`.

Make sure to save this evidence to the satchel and update the `Analyst Threadboard`:

![Ash & Wick Register Threadboard](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/threadboard-3.png)


### Register - Quiet Mercy Holdings Ltd

The analyst briefing directed us to find the company behind `VR-118204` (`Ash & Wick Provisioners Ltd`). We now know `Quiet Mercy Holdings Ltd` own a 100% stake in `Ash & Wick`, let's look them up in the register.

![Register](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/qmh-register.png)

|:---------------------:|:------------:|
| Registered Office     | 14 Cinder Lane, Eastreach
| Directory             | `Nera Sorn` and `Ilyra Venn`
| Company Secretary     | Vey & Marr Corporate Services
| Shareholder           | Crown Road Fiduciary Foundation - 100%


Both `Ash & Wick Provisioners Ltd` and `Quiet Mercy Holdings Ltd` have the same directory `Nera Sorn` and the same registered office.

Make sure to save this evidence to the satchel and update the `Analyst Threadboard`:

![Threadboard Complete](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/threadboard-complete.png)

### Findings Submission

In `Submit`, we can use all the evidence gathered to submit the answers to the questions.

![Questions Answered 1](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/submission-complete.png)
![Questions Answered 2](/assets/img/2026-posts/cyber-apocalypse/the-helpers-with-clean-hands/submission-complete-2.png)

> The charity teaches mercy. The company moves cord. The holding company appoints the hands that sign both sets of paper. Miren does not find an army. She finds the people who make one possible.

Our briefing tasked us with finding the company behind `VR-118204` (`Ash & Wick Provisioners Ltd`). Throughout this challenge, we were able to use the `Register` and `Tenders` to show `Quiet Mercy Holdings Ltd` holds a 100% stake in `Ash & Wick Provisioners Ltd`. We also learned both companies share the same director `Nera Sorn`.

The challenge scenario specified the flag format is: `HTB{SUPPLIER_FEEDS_TOWN}`. 
* The supplier of `Mercy Lantern Relief Trust` was `Ash & Wick Provisioners`. 
* The town being supplied was `Harrowgate`. 

Putting the clues together leaves us with the following flag: `HTB{ASH_AND_WICK_FEEDS_HARROWGATE}`.

