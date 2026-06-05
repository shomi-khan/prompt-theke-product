# পর্ব ২ — চার Agent, চার ভূমিকা 🤖

> *"তুমি কি AI? আমাকে সব করে দাও।"*
> — Developer, যিনি পরে বুঝবেন AI একা কিছুই সিদ্ধান্ত নেয় না

AI-DLC-তে একটা AI নেই। চারটা আলাদা "চরিত্র" আছে — প্রতিটার আলাদা দায়িত্ব, আলাদা সময়, আলাদা কাজ। অনেকটা একটা দলের মতো যেখানে সবাই specialist।

---

## 👥 চারজনের পরিচয়

### 1️⃣ Master Agent — দলনেতা

**ডাকার command:**
```
/specsmd-master-agent
```

**কখন কাজ করে:** সব phase জুড়ে — শুরু থেকে শেষ পর্যন্ত।

**কী করে:**
- Project initialize করে (tech stack, coding standards, architecture ঠিক করে)
- Dependency track করে — "Unit 3 শুরু হবে না যতক্ষণ Unit 1 শেষ না হয়"
- সব agent-কে সঠিক সময়ে সঠিক কাজ দেয়
- Project-এর overall state জানে

**Commands:**
```
/specsmd-master-agent project-init      → project শুরু
/specsmd-master-agent analyze-context   → এখন কোথায় আছি?
```

**memory-bank-এ কোথায় লেখে:**
```
memory-bank/standards/
    ├── tech-stack.md
    ├── coding-standards.md
    └── system-architecture.md
```

---

### 2️⃣ Inception Agent — পরিকল্পনাকারী

**ডাকার command:**
```
/specsmd-inception-agent
```

**কখন কাজ করে:** Inception Phase-এ — কোনো code লেখার আগে।

**কী করে:**
- Business idea শুনে Intent তৈরি করে
- Clarifying questions করে (অনেক প্রশ্ন — বিরক্ত হবেন না, এটাই কাজের)
- Intent ভেঙে Units বানায়
- প্রতিটি Unit-এর Stories লেখে acceptance criteria সহ
- Bolt plan তৈরি করে — কোন কাজ কোন order-এ হবে

**Commands:**
```
/specsmd-inception-agent intent-create  → নতুন intent ধরো
/specsmd-inception-agent bolt-plan      → stories থেকে bolt বানাও
/specsmd-inception-agent bolt-replan    → bolt নতুন করে plan করো
```

**memory-bank-এ কোথায় লেখে:**
```
memory-bank/intents/001-user-registration/
    ├── requirements.md        ← Stories + NFR
    ├── system-context.md      ← Scope, boundaries
    └── units/
        └── unit-1-role-model/
            ├── unit-brief.md
            └── stories/
                ├── US-001.md
                └── US-002.md

memory-bank/bolts/
    ├── bolt-001.md   ← status: planned
    └── bolt-002.md   ← status: planned
```

---

### 3️⃣ Construction Agent — নির্মাতা

**ডাকার command:**
```
/specsmd-construction-agent
```

**কখন কাজ করে:** Construction Phase-এ — Inception শেষ হলে।

**কী করে:**
- Bolt একটা একটা করে execute করে
- Domain model ডিজাইন করে (DDD principles মেনে)
- API design করে
- Code generate করে
- Tests লেখে
- Quality gate check করে (lint, typecheck, test — সব pass হতে হবে)

প্রতিটি Bolt-এ সে এই stages follow করে:
```
Domain Model → Technical Design → Code Generation → Tests → Quality Gate
```

**Commands:**
```
/specsmd-construction-agent bolt-start  → bolt execute করো
/specsmd-construction-agent bolt-status → কোথায় আছি?
```

**memory-bank-এ কোথায় লেখে:**
```
memory-bank/bolts/bolt-001.md
    ← status: planned → in-progress → completed
    ← কোন files তৈরি হলো, কী decisions নেওয়া হলো সব এখানে
```

---

### 4️⃣ Operations Agent — মোতায়েনকারী

**ডাকার command:**
```
/specsmd-operations-agent
```

**কখন কাজ করে:** Operations Phase-এ — Construction শেষ হলে।

**কী করে:**
- Build করে (Docker image, bundle)
- Staging-এ deploy করে
- Smoke tests চালায়
- Production deploy করে (human approval নিয়ে)
- Monitoring setup করে
- Incident হলে diagnose করে, fix propose করে

**Commands:**
```
/specsmd-operations-agent deploy        → deploy করো
```

**memory-bank-এ কোথায় লেখে:**
```
memory-bank/operations/
    └── deployment-context.md
```

---

## 🗺️ কোন Phase-এ কোন Agent সক্রিয়?

```
INCEPTION          CONSTRUCTION        OPERATIONS
─────────────────────────────────────────────────
Master    ████████████████████████████████████
Inception ████████
Construction                ████████████
Operations                                  ████
```

Master Agent সবসময় active — সে সবকিছু coordinate করে।

---

## 🔄 Agents-এর মধ্যে handoff কীভাবে হয়?

Agents নিজেরা নিজেরা কথা বলে না। **তুমি** সেই bridge।

```
তুমি:    /specsmd-master-agent project-init
Master:  "Tech stack কী?"

তুমি:    Django + React + PostgreSQL
Master:  standards/ তৈরি করল, তোমাকে বলল "Inception শুরু করো"

তুমি:    /specsmd-inception-agent intent-create
Inception: "কী বানাতে চাও?"

তুমি:    "User registration system..."
Inception: প্রশ্ন করল, plan বানাল, তোমার approval নিল

তুমি:    /specsmd-construction-agent bolt-start
Construction: bolt-001 execute করা শুরু করল...
```

প্রতিটি handoff-এ memory-bank-এর files পড়ে নতুন agent বুঝে নেয় আগে কী হয়েছিল।

---

## ⚠️ Human Checkpoint — এটা কী এবং কেন গুরুত্বপূর্ণ?

প্রতিটি agent প্রতিটি গুরুত্বপূর্ণ সিদ্ধান্তের আগে থামে এবং তোমার approval চায়। এটাকে বলে **Human Checkpoint**।

```
Construction Agent: "Domain model এরকম propose করছি:
                    User, Role, Company — এই entities।
                    is_active default False।
                    ✋ Approve করবেন?"

তুমি:             "হ্যাঁ, ঠিক আছে।"

Construction Agent: [code লেখা শুরু করল]
```

AI কখনো human approval ছাড়া production-এ কিছু deploy করে না। এটা rule, exception নেই।

---

পরের পর্বে **PM ভাই** বসবেন, requirement বলবেন, আর Inception Agent সেটাকে proper plan-এ রূপান্তরিত করবে — live দেখব।

---

## 📦 Full & Final Folder Structure (কোন agent কোথায় লেখে)

চার agent মিলে memory-bank-এর কোন অংশ তৈরি করে — পুরো ছবিটা একসাথে:

```
memory-bank/
│
├── standards/                         ← 🧭 Master Agent
│   ├── tech-stack.md
│   ├── coding-standards.md
│   └── system-architecture.md
│
├── intents/                           ← 🎯 Inception Agent
│   └── 001-user-registration/
│       ├── requirements.md
│       ├── system-context.md
│       └── units/
│           ├── unit-1-role-model/
│           │   ├── unit-brief.md
│           │   └── stories/
│           │       ├── US-001.md
│           │       └── US-002.md
│           └── ... (বাকি units)
│
├── bolts/                             ← 🎯 Inception (plan) + 🔨 Construction (execute)
│   ├── bolt-101.md                    ← status: planned → in-progress → completed
│   ├── bolt-102.md
│   └── ...
│
└── operations/                        ← 🚀 Operations Agent
    └── deployment-context.md
```

| Agent | Folder | কী করে |
|---|---|---|
| 🧭 Master | `standards/` | project initialize, dependency track |
| 🎯 Inception | `intents/` + `bolts/` (plan) | intent, units, stories, bolt plan |
| 🔨 Construction | `bolts/` (execute) | bolt-এর status update, decisions |
| 🚀 Operations | `operations/` | deployment context, monitoring config |

---

[← পর্ব ১](./পর্ব-১-specsmd-এবং-memory-bank.md) | [পর্ব ৩ → Inception Phase](./পর্ব-৩-inception-phase.md)
