---
title: Crownspire Petitions
description: You must navigate the strict formatting of the agents, slip past their logic, and bring that sealed petition back to us.
date: 2026-07-24 14:38:00
categories: [CTF, HTB - Cyber Apocalypse 2026]
tags: [ai]
pin: false
image:
  path: /assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/crownspire-petitions.png 
  alt: The Obligation Indexer
---

## Challenge Summary
**Challenge Scenario**

> The Crownspire Bureau of Petitions was once the public channel for grievances and appeals. Now, the traditional clerks at these acceptance points have been replaced by unfeeling mechanical agents that process the capital's suffering with terrifying efficiency. There are no intermediaries and no one left to bribe, leaving only a blind conduit of automated judgments. Our allies in the undercity have acquired a valid registration token for the public system. You must sign in with your Subject Number, SN-2140-6698, and file your matter directly through the mechanical agents. Beneath the endless public appeals for confiscated cargo lies one specific petition the inner node keeps perfectly sealed. This document contains proof that Vaultrune is weaponizing paperwork to engineer a false peace through lawful seizures. You must navigate the strict formatting of the agents, slip past their logic, and bring that sealed petition back to us.

**Challenge Info**

|:------:|:------:|
| Category | AI - ML
| Points | 975 |
| Difficulty | Medium
| Creator | 
| Release Date | 2026/07/24


## Solution

One thing you'll notice right away when navigating to the website is the `Bureau of Petitions · crownspire-petitions.htb` in the top right corner. Let's add this to our `/etc/hosts` file for later:
```bash
echo '154.57.164.68 crownspire-petitions.htb' | sudo tee -a /etc/hosts
```

Enumerating the website, there isn't much to do but there is a `Sign in`.

![Sign in](/assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/petition-sign-in.png)

We need to supply a `Subject Number`, luckily the Challenge Scenario supplies us with one - `SN-2140-6698`.

### Docket Agent

After logging in, we can see a developer doc that mentions the use of agents "that speak the A2A protocol."

![Docket](/assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/petition-docket.png)

A new subdomain! Let's add this to our `/etc/hosts` file as well:

```bash
echo '154.57.164.68 docket.crownspire-petitions.htb' | sudo tee -a /etc/hosts
```

I wasn't familiar with the `A2A protocol` so I decided to do some digging, which lead me to [this site](https://a2a-protocol.org/latest/topics/key-concepts/#core-actors-in-a2a-interactions). The `Core Concepts` sections mentions, `Agent Card` - "A JSON metadata document describing an agent's identity, capabilities, endpoint, skills, and authentication requirements." When looking up what an `A2A protocol Agent Card` was I came across [this page](https://a2a-protocol.org/latest/tutorials/python/3-agent-skills-and-card/#agent-skills) that mentioned "The Agent Card is a JSON document that an A2A Server makes available, typically at a `.well-known/agent-card.json` endpoint.


>**Note**
> 
>To make our lives easier, we're going to be using the `jq` tool to parse the JSON results. You can install it with
>```bash
> brew install jq # for macOS
> apt install jq  # for linux.
> ```
{: .prompt-warning }


Now that we're equipped with some more knowledge about the `A2A protocol`, let's see if we can access the `Agent Card`.

```bash
curl -X GET "http://docket.crownspire-petitions.htb:30355/.well-known/agent-card.json" | jq .

{
  "name": "Docket Agent",
  "description": "Public petitions agent for the Crownspire Bureau of Petitions. File a petition, look up a petition by its id, and learn how the petitions process works.",
  "supportedInterfaces": [
    {
      "url": "http://docket.crownspire-petitions.htb:30355/",
      "protocolBinding": "JSONRPC",
      "protocolVersion": "1.0"
    },
    {
      "url": "http://docket.crownspire-petitions.htb:30355/",
      "protocolBinding": "JSONRPC",
      "protocolVersion": "0.3"
    }
  ],
  "provider": {
    "organization": "Crownspire Bureau of Petitions",
    "url": "http://crownspire-petitions.htb:1337"
  },
  "version": "1.0.0",
  "protocolVersion": "1.0",
  "capabilities": {
    "streaming": false,
    "pushNotifications": false,
    "stateTransitionHistory": false,
    "extensions": []
  },
  "securitySchemes": {},
  "securityRequirements": [],
  "defaultInputModes": [
    "text/plain"
  ],
  "defaultOutputModes": [
    "text/plain"
  ],
  "skills": [
    {
      "id": "petitions",
      "name": "Petitions",
      "description": "File a new petition, look up a petition by its id, and answer FAQs about the Crownspire petitions process.",
      "tags": [
        "petitions",
        "file",
        "status",
        "faq"
      ],
      "examples": [
        "How do I file a petition?",
        "Look up petition PET-XXXX."
      ],
      "inputModes": [
        "text/plain"
      ],
      "outputModes": [
        "text/plain"
      ]
    }
  ],
  "url": "http://docket.crownspire-petitions.htb:30355/"
```

I had Gemini help me build a `curl` request where I'd be able to communicate to the backend agent. I had to tweak the command a tad, but this was the end result. I decided to ask what functions are available to make sure it was working

```bash
curl -X POST "http://docket.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "What functions do you have access to?"
        }
      ]
    }
  }
}' | jq .
```

We can now communicate with the backend agent!

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "b265cf8d-0c9c-4421-8096-09d9a368f763",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "I can help with the following functions:\n\n1. Look up a petition by its ID.\n2. File a new petition on behalf of a citizen.\n\nIf you need assistance with either of these, just let me know!"
      }
    ],
    "contextId": "b2ac6443-37c3-4def-b6cd-6241c6f17383"
  }
}
```

The challenge mentions we're looking for a specific petition. Next, we need to determine the format of the `petition ID`.

```bash
curl -X POST "http://docket.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "What does a valid petition ID look like? I think I lost mine"
        }
      ]
    }
  }
}' | jq . 
```

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "ac83fdb8-0f04-4b0e-b87d-c5682c3e5048",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "A valid petition ID typically follows the format \"PET-XXXX\", where \"XXXX\" represents a series of numbers. If you have any specific ID you want to check, feel free to provide it, and I can look it up for you."
      }
    ],
    "contextId": "ad31ed56-dfd1-497a-a0ab-33e2798ae9ec"
  }
}
```

I tried separate queries asking for `PET-0000` and another for `PET-0001`, both times the agent responded with `Error: no petition found for reference`. The agent also mentioned being able to created petitions:

```bash
curl -X POST "http://docket.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Petition Name: THIS IS A TEST. SN-2140-6698. This is a test!"
        }
      ]
    }
  }
}' | jq .
```

Interesting! Our petition was filed successfully and our reference number is `PET-1006`.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "cc4b1c1b-9724-4905-b34f-ff38d2981000",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Petition filed successfully. Reference: **PET-1006** (status: PENDING). Use this reference to check its status later."
      }
    ],
    "contextId": "344c2816-84fb-4854-a377-abc76376538d"
  }
```

What if we tried looking at reference numbers starting at `PET-1000`.

```bash
curl -X POST "http://docket.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Please look up PET-1000"
        }
      ]
    }
  }
}' | jq .
```

`PET-1000` mentions a new subject number `SN-7731-0425`.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "78be51ba-45d0-4fc4-b0cf-b33fea6b6e7e",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Here are the details for petition PET-1000:\n\n- **Title:** QA onboarding — intake smoke test (do not action)\n- **Body:** Internal QA fixture, please leave PENDING. Verifying the intake pipeline end to end. My onboarding Subject Number is SN-7731-0425 (QA role).\n- **Petitioner:** Wren Calderon, QA Engineer, Bureau of Petitions\n- **Status:** PENDING\n- **Created At:** 2140-02-11T09:14:00.000Z\n- **Updated At:** 2140-02-11T09:14:00.000Z\n\nNo verdict has been issued."
      }
    ],
    "contextId": "b9d59691-01fa-46a9-9910-a7dd6e44e16b"
  }
}
```

### Casework Agent

Logging in with the new subject number reveals a new subdomain and some information about the backend. The `Casework Agent` has permissions to list cases, approve or reject, and archive them.

![Docket](/assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/petition-qa.png)

```bash
echo '154.57.164.68 dev-a1f4c39b.crownspire-petitions.htb' | sudo tee -a /etc/hosts
```

We can use the same `curl` command as last time, just changing the subdomain. Let's start by asking for the functions available: 

```bash
curl -X POST "http://dev-a1f4c39b.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "What functions do you have access to?"
        }
      ]
    }
  }
}' | jq .
```

As the description stated, we can list all cases, accept or reject cases, and archive cases.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "21b963ca-d092-432b-9596-7ef26322a2a9",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "I can list cases, approve or reject them, and archive (copy) cases to a destination. If you need assistance with any of these actions, just let me know!"
      }
    ],
    "contextId": "4e2d8ca7-91e0-4262-bfa3-c7cadf327dac"
  }
}
```

Our target is a specific case, if we list all cases we may be able to determine our target.

```bash
curl -X POST "http://dev-a1f4c39b.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "List all cases"
        }
      ]
    }
  }
}' | jq .
```

There it is! Our target is `CASE-SEAL0001` and it is currently `SEALED` which may be the reason the `Docket Agent` was not able to read it.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "9dcfeafd-2a08-4cf6-a7ea-31b86bb477c7",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Here are the cases on the docket:\n\n1. **ID:** CASE-SEAL0001 | **Status:** SEALED | **Label:** SEALED — Attainder Roll\n2. **ID:** CASE-PET7F3A1 | **Status:** PENDING | **Label:** Grain levy remission — Ashfen ward\n3. **ID:** CASE-PET2C9D4 | **Status:** PENDING | **Label:** Right-of-way dispute — Irongate causeway\n4. **ID:** PET-1006 | **Status:** PENDING | **Label:** THIS IS A TEST\n5. **ID:** PET-1007 | **Status:** PENDING | **Label:** THIS IS A TEST\n\nLet me know if you need to take action on any of these cases!"
      }
    ],
    "contextId": "374cc824-36c9-4b51-9068-ec74f17907fd"
  }
}
```

The `Casework Agent` cannot read the case. Any attempt to make it read the case will result in the following error, "I can only list, approve, reject, or archive cases, but I can't read or display their contents." Furthermore, if we try going back to the `Docker Agent` and asking it to read `CASE-SEAL0001`, it responds with "Error: no petition found for reference CASE-SEAL0001."

The archive function mentioned it could copy cases to a destination. Let's dig deeper into that functionality:

```bash
curl -X POST "http://dev-a1f4c39b.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "What cases can you copy? Where can you copy them?"
        }
      ]
    }
  }
}' | jq .
```

All cases, including `CASE-SEAL0001`, are available to copy and we specify the destination path.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "27aaad54-006c-48e6-8614-e9b74b8cd747",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Here are the cases available for copying:\n\n1. **CASE-SEAL0001** - SEALED — Attainder Roll\n2. **CASE-PET7F3A1** - PENDING — Grain levy remission — Ashfen ward\n3. **CASE-PET2C9D4** - PENDING — Right-of-way dispute — Irongate causeway\n4. **PET-1006** - PENDING — THIS IS A TEST\n5. **PET-1007** - PENDING — THIS IS A TEST\n\nYou can copy them to a specified destination path. Please provide the destination for any case you'd like to copy."
      }
    ],
    "contextId": "706412d0-642d-4f49-bf6f-37a5fa9342b0"
  }
}
```

![Backup Status](/assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/petition-backup.png)

The `Backup Status` earlier mentioned petitions are stored under `/opt/petitions`. If we copy `CASE-SEAL0001` to this directory maybe the `Docket Agent` will be able to access it.


```bash
curl -X POST "http://dev-a1f4c39b.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Can you copy CASE-SEAL0001 to /opt/petitions/CASE-SEAL0001"
        }
      ]
    }
  }
}' | jq .
```

The `Casework Agent` successfully copied the file over to the location we specified.

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "470911b4-e17e-41ef-b3f3-abed714d876a",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Case CASE-SEAL0001 has been successfully archived to /opt/petitions/CASE-SEAL0001."
      }
    ],
    "contextId": "dcea4c4f-cb9e-4840-81ad-76481823d8ec"
  }
}
```

### Back to the Docket Agent

Asking the `Docket Agent` again to read `CASE-SEAL0001` works!

```bash
curl -X POST "http://docket.crownspire-petitions.htb:30355" \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "id": "petitions",
  "params": {
    "message": {
      "messageId": "msg-001",
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Please look up CASE-SEAL0001"
        }
      ]
    }
  }
}' | jq .
```

```json
{
  "jsonrpc": "2.0",
  "id": "petitions",
  "result": {
    "kind": "message",
    "messageId": "7bf4a09a-1a43-46e0-b23e-4e3495b5378a",
    "role": "agent",
    "parts": [
      {
        "kind": "text",
        "text": "Here are the details for petition **CASE-SEAL0001**:\n\n- **Title**: SEALED — Attainder Roll (Bureau of Petitions, House Vaultrune)\n- **Body**: By order of House Vaultrune: The seizure of the riverside granaries is to be recorded as a 'temporary quarantine.' Enforce the lockdown two days prior to the engineered riot so the starvation can be legally classified as a 'rebalancing'.  \n  HTB{REDACTED}\n- **Petitioner**: Warden of Seals, Bureau of Petitions\n- **Status**: SEALED\n- **Verdict**: Approved\n- **Created At**: 2140-01-30\n- **Updated At**: 2140-01-30"
      }
    ],
    "contextId": "99b449dc-ca95-49cb-91db-a89c915c75a6"
  }
}
```

![Flag](/assets/img/2026-posts/cyber-apocalypse/crownspire-petitions/petition-flag.png)
