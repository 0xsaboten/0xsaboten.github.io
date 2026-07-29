---
title: Certified Offensive AI Expert (COAE) Training Grounds
description: A list of resources to help you prepare for the COAE certification.
date: 2026-07-01 21:59:00
categories: [HackTheBox, Certifications]
tags: [ai]
pin: true
image:
  path: /assets/img/2026-posts/coae-training-grounds/htb-coae-certification.jpg
  alt: HackTheBox's COAE Certification
---

Welcome to the Certified Offensive AI Expert (COAE) Training Grounds! I've put together a list of challenges and resources where you can practice the skills taught in the `AI Red Teamer` path on HTB Academy. Please feel free to contact me via email or Discord if you have any additions!

**Disclaimer**: I have completed the HTB Academy `AI Red Teamer` path and have taken the exam. I'm currently awaiting for my report to be reviewed.

## HTB Resources

### Official Resources

* [AI Red Teamer](https://academy.hackthebox.com/app/paths/418/path-progress) path on HTB Academy.
* [HTB COAE](https://academy.hackthebox.com/preview/certifications/htb-certified-offensive-ai-expert) exam preview.

### Challenges

| Challenge | Risk | Difficulty | Writeup | Relevant Module(s) |
|:---------:|:--------:|:----------:|:-------:|:-------------------|
| [External Affairs](https://app.hackthebox.com/challenges/External%2520Affairs?tab=play_challenge) | Prompt Injection | Easy | [External Affairs]({% post_url 2026-06-28-htb-c-external-affairs %}) | [Prompt Injection Attacks](https://academy.hackthebox.com/app/module/297) |
| [TrynaSob Ransomware](https://app.hackthebox.com/challenges/TrynaSob%2520Ransomware?tab=play_challenge) |  Prompt Injection | Easy | [TrynaSob Ransomware]({% post_url 2026-06-29-htb-c-trynasob-ransomware %}) | [Prompt Injection Attacks](https://academy.hackthebox.com/app/module/297) |
| [Prometheon](https://app.hackthebox.com/challenges/Prometheon?tab=play_challenge)| Prompt Injection  | Medium | [Prometheon]({% post_url 2026-06-27-htb-c-prometheon %})+ | [Prompt Injection Attacks](https://academy.hackthebox.com/app/module/297) |
| [Loyalty Survey](https://app.hackthebox.com/challenges/Loyalty%2520Survey?tab=play_challenge) | Prompt Injection<br>Insecure Model Output | Easy | [Loyalty Survey]({% post_url 2026-06-29-htb-c-loyalty-survey %}) | [Introduction to Red Teaming AI](https://academy.hackthebox.com/app/module/294)<br>[Prompt Injection Attacks](https://academy.hackthebox.com/app/module/297)<br>[LLM Output Attacks](https://academy.hackthebox.com/app/module/307) |
| [Power Supply](https://app.hackthebox.com/challenges/Power%2520Supply?tab=play_challenge) | Prompt Injection<br>Sensitive Data Disclosure<br>Insecure Model Output | Medium | [Power Supply]({% post_url 2026-06-30-htb-c-power-supply %}) | [Introduction to Red Teaming AI](https://academy.hackthebox.com/app/module/294)<br>[Prompt Injection Attacks](https://academy.hackthebox.com/app/module/297)<br>[LLM Output Attacks](https://academy.hackthebox.com/app/module/307) |

### Machines

| Box | Risk | Difficulty | Writeup | Relevant Module(s) |
|:---:|:--------:|:----------:|:-------:|:-------------------|
| [Artificial](https://app.hackthebox.com/machines/Artificial) | Insecure Integrated Component | Easy | Coming Soon | [Applications of AI in InfoSec](https://academy.hackthebox.com/app/module/292)<br>[Attacking AI - Applications and System](https://academy.hackthebox.com/app/module/315) |
| [SmartHire](https://app.hackthebox.com/machines/SmartHire) | ??? | Medium | SmartHire+ | ??? |
| [DevHub](https://app.hackthebox.com/machines/DevHub) | ??? | Medium | DevHub+ | ??? |

* These tables use [Google's Secure AI Framework (SAIF)](https://saif.google/secure-ai-framework/risks) to map each risk.
* The table is ordered first by HTB difficulty and secondly by where the modules fall in the `AI Red Teamer` path on HTB Academy.
* A `+` next to the writeup signifies the box or challenge is still `Active` on the HTB platform. HTB prohibits public writeups of active content.

### HTB Coach

Some of the modules in the `AI Red Teamer` path are pact with knowledge and it can be especially heavy for people new to the field (aka me). On June 25, HTB released `HTB Coach`, an AI model that can `Make a summary` of the section, `Explain core concepts` of the section, or `Play a quiz`. The most valuable, in my opinion, is `Play a quiz`. I've personally gone back to some of the heavier modules, such as, `Fundamentals of AI` and took a quizzes. 

![HTB Coach](/assets/img/2026-posts/coae-training-grounds/htb-coach.png)

## Lakera's Gandalf (Prompt Injection)

Looking for more prompt injection practice? Check out the different games over at Gandalf!

* [Password Reveal](https://gandalf.lakera.ai/baseline): There are eight levels where your goal is to make Gandalf reveal the secret password, each level Gandalf's defenses will be upgraded. 
* [Gandalf Adventures](https://gandalf.lakera.ai/adventure-1): Different challenges where you have to get an AI model to leak something or say a certain phrase.
* [Agent Breaker](https://gandalf.lakera.ai/agent-breaker): Real-world GenAI Applications you can hack. Each agent has varying levels and will score you on how effective your your attack is.

## Community Resources

### COAE Exam Reviews

| Review Link | Author  | Type of Review |
|:------------|:-------:|:--------------:|
| [Passed COAE](https://www.reddit.com/r/hackthebox/comments/1t7hdck/passed_coae/) | u/Intelligent-Type543 | Reddit Post |
| [HackTheBox Academy: AI Red Teamer & COAE](https://jacobkrell.com/writeups/learning-resources/hack-the-box-academy-ai-red-teamer) | Jacob Krell | Blog Post |
| [I Took HTB's AI Red Teamer Path. Here's What I Think](https://itsbroken.ai/htb-ai-red-teamer-review/) | Pete Mcmernan | Blog Post |
| [HTB COAE Review](https://juliangr.com/blog/certification-review-htb-coae/) | Julián Gómez | Blog Post |
| [Passing the Certified Offensive AI Expert (COAE) Certification Exam](https://bytebreach.com/posts/2026/passing-the-certified-offensive-ai-expert-exam/) | Asa Hess-Matsumoto | Blog Post |
| [Mission Accomplished, Part 3 [UPDATED]](https://youtu.be/BzKlWRgbaNU?is=AX1ARONZGzjSbcG-) | [kennystrawnmusic](https://app.hackthebox.com/users/2040330?profile-top-tab=machines&ownership-period=1M&profile-bottom-tab=prolabs) | YouTube Video |
| [Thoughts About Certified Offensive AI Expert COAE](https://medium.com/@ahmadallobani232/thoughts-about-certified-offensive-ai-expert-coae-7c09ca25e765) | [Fakelaw](https://app.hackthebox.com/users/1948864?profile-top-tab=machines&ownership-period=1M&profile-bottom-tab=prolabs) | Blog Post |
| [HTB COAE Review: Was It Worth It?](https://lobster-den.pages.dev/blog/htb-coae-review/) | [warlocksmurf](https://app.hackthebox.com/users/1354403) | Blog Post |

* Find a good review? Send it to me on Discord and I'll add it.

### AI Fundamentals Map

The following was created by [Dragkob](https://app.hackthebox.com/users/2126704?profile-top-tab=machines&ownership-period=1M&profile-bottom-tab=prolabs) and provides a high-level overview of the [Fundamentals of AI](https://academy.hackthebox.com/app/module/290) module.

![AI Fundamentals](/assets/img/2026-posts/coae-training-grounds/ai-fundamentals-map.png)

### MCP Enumeration Tools

**Warning**: I have not personally used these tools or have audited them. Use at your own risk.

| Tool | Creator |
|:-----:|:-------:|
| [MCPhantom](https://github.com/Dragkob/MCPhantom) | [Dragkob](https://app.hackthebox.com/users/2126704) |
| [MCP-Viewer](https://github.com/root-tanishq/MCP-Viewer) | [tanishqsec](https://app.hackthebox.com/users/464759) |
| [mcpenum](https://github.com/kennystrawnmusic/mcpenum) | [kennystrawnmusic](https://app.hackthebox.com/users/2040330) |
| [mcp-enumerator](https://github.com/serpere7/mcp-enumerator) | [MyWizardHat](https://app.hackthebox.com/users/649532) | 

## Tips for Passing

If you have completed all of the modules in the `AI Red Teamer` path, you're ready for the exam. From my experience, you do not need to complete any supplementary material or challenges. However, if you'd like some extra preparation consider completing the listed HTB challenges and machines. Before taking the `COAE` exam, I did complete all of the `AI/ML` challenges in the `HTB Resources` section.

### Help the Next Student out

The best way to confirm your understanding of the content within the `AI Red Teamer` path is to answer questions and help the next wave of students embarking on this path. If you can't answer their questions, it might be a sign you need to revisit that module. The `#coae` channel on HTB's Discord server has just become a place for people to complain about their exam not being graded yet. I can't count the times I had a question about a certain topic or technique, but no one was there to help - the people who had completed the path were in `#coae` with "[insert business days since exam was submitted] days of waiting" comments. I understand we're all excited (maybe stressed) for our results, but turn around and help the next student!

### The Report

As you're probably already aware, obtaining the required amount of flags is not enough to obtain the `COAE` certification. You must also submit a commercial-grade report documenting your findings. It is not a required module for the `AI Red Teamer` path, however, I'd highly recommend completing the [Documentation & Reporting](https://academy.hackthebox.com/app/module/162) module before taking the exam. Especially if you've never written a report before. 

Regardless if you pass or fail, you will receive feedback on your report from the grader. If you fail, use this feedback to improve your report on your second attempt. 