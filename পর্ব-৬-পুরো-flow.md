# পর্ব ৬ — পুরো Flow একনজরে 🗺️

> *"তাহলে আমরা কী কী করলাম?"*
> — তুমি, এই পর্বে আসার পর

অনেক কিছু হলো। এবার সব একসাথে দেখি।

---

## 🎬 গল্পের সারসংক্ষেপ

**তিনজন ছিলেন:**
- 🧑‍💼 Rafiq ভাই — PM
- 👨‍💻 Sami — Backend Developer
- 👩‍💻 Nila — Frontend Developer

**একটা feature ছিল:**
- User Registration System (Django + React, 3 flows, 3 roles)

**একটা methodology ছিল:**
- AI-DLC

**আর একটা AI ছিল, যে:**
- প্রশ্ন করেছে
- Plan বানিয়েছে
- Code লিখেছে
- Deploy করেছে
- কিন্তু কোনো সিদ্ধান্ত একা নেয়নি

---

## 🗺️ পুরো Journey

```
📋 INCEPTION PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Master Agent
  └─ project-init
      ├─ tech-stack.md ✅
      ├─ coding-standards.md ✅
      └─ system-architecture.md ✅

Inception Agent
  └─ intent-create
      ├─ Rafiq ভাই requirement বললেন
      ├─ AI 5টি clarifying question করল
      ├─ requirements.md ✅
      └─ system-context.md ✅

  └─ unit decomposition
      └─ 8 Units defined ✅

  └─ story writing
      └─ 24 Stories written ✅

  └─ bolt-plan
      └─ 18 Bolts planned ✅

⛔ Final Inception Approval → PASSED

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔨 CONSTRUCTION PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Construction Agent (Bolt by Bolt)

  Unit 1: Role & Permission Model
    Bolt 1.1 → User Model ✅
    Bolt 1.2 → Permissions ✅

  Unit 2: Invitation Core Engine
    Bolt 2.1 → Status Machine ✅  ← সবচেয়ে critical
    Bolt 2.2 → Email Service ✅
    Bolt 2.3 → Token Validation API ✅

  Unit 3: Flow 1
    Bolt 3.1 → Backend API ✅
    Bolt 3.2 → Frontend UI ✅

  Unit 4: Flow 2
    Bolt 4.1 → APIs ✅
    Bolt 4.2 → Frontend ✅

  Unit 5: Flow 3
    Bolt 5.1-5.4 → APIs + Frontend ✅

  Unit 6: Shared Invitation Page
    Bolt 6.1 → Details Page ✅
    Bolt 6.2 → Error Pages ✅

  Unit 7: Password Setup
    Bolt 7.1-7.2 → API + UI ✅

  Unit 8: Role Promotion
    Bolt 8.1-8.2 → API + UI ✅

⛔ Construction Final Gate → PASSED

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 OPERATIONS PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Operations Agent

  Build ✅
  Staging Deploy ✅
  ⛔ Client Demo → APPROVED
  Production Deploy (Blue/Green) ✅
  48h Monitoring ✅
  ⛔ Final Sign-off → DONE
```

---

## 📁 memory-bank Final State

```
memory-bank/
├── standards/
│   ├── tech-stack.md              ✅
│   ├── coding-standards.md        ✅
│   └── system-architecture.md     ✅
│
├── intents/
│   └── 001-user-registration/
│       ├── requirements.md        ✅ (stories, NFR, clarifications)
│       ├── system-context.md      ✅
│       └── units/
│           ├── unit-1/  ✅
│           ├── unit-2/  ✅
│           ├── unit-3/  ✅
│           ├── unit-4/  ✅
│           ├── unit-5/  ✅
│           ├── unit-6/  ✅
│           ├── unit-7/  ✅
│           └── unit-8/  ✅
│
├── bolts/
│   ├── bolt-101.md  ✅ completed
│   ├── bolt-102.md  ✅ completed
│   ├── bolt-201.md  ✅ completed
│   ... (18টি, সব completed)
│
└── operations/
    └── deployment-context.md  ✅
```

---

## ✅ AI-DLC Checklist

নিজের project-এ AI-DLC follow করতে চাইলে এই checklist ব্যবহার করো:

### Inception Phase

```
□ npx specsmd@latest install চালানো হয়েছে
□ /specsmd-master-agent project-init complete
□ tech-stack.md, coding-standards.md তৈরি হয়েছে
□ /specsmd-inception-agent intent-create চালানো হয়েছে
□ সব clarifying questions-এর উত্তর দেওয়া হয়েছে
□ Units approved হয়েছে
□ Stories + acceptance criteria লেখা হয়েছে
□ Bolt plan approved হয়েছে
□ Final Inception gate pass হয়েছে
□ কোনো code এখনো লেখা হয়নি 😄
```

### Construction Phase

```
□ /specsmd-construction-agent bolt-start চালানো হয়েছে
□ প্রতিটি bolt-এ domain model approved হয়েছে
□ প্রতিটি bolt-এ technical design approved হয়েছে
□ প্রতিটি bolt-এ code plan approved হয়েছে
□ Quality gate (test + lint + typecheck) pass হয়েছে
□ Bolt status updated হয়েছে memory-bank-এ
□ সব bolts completed
□ Final Construction gate pass হয়েছে
```

### Operations Phase

```
□ /specsmd-operations-agent deploy চালানো হয়েছে
□ Build artifacts clean
□ Staging deploy হয়েছে
□ Smoke tests pass হয়েছে
□ Client/PM staging দেখেছেন এবং approve করেছেন
□ Production deploy approved হয়েছে
□ Blue/Green deployment complete
□ Monitoring + alerts setup হয়েছে
□ 48h watch period শেষ
□ Final sign-off নেওয়া হয়েছে
```

---

## 🔑 Key Takeaways

**১. Code-এর আগে clarity।**
Inception Phase-এ প্রশ্ন করতে বিরক্ত হলে চলবে না। একটা unresolved question = production-এ bug।

**২. AI একা চলে না।**
Human Checkpoint মানে formality না — এটাই system-এর safety net।

**৩. memory-bank = team-এর shared brain।**
PM, Backend, Frontend — সবাই একই files পড়ে। কেউ "আমি জানতাম না" বলতে পারবে না।

**৪. Bolt ছোট রাখো।**
একটা Bolt যদি ২ দিনের বেশি লাগে, সেটাকে ভাঙো। ছোট bolt = ছোট risk।

**৫. Quality gate skip করা যাবে না।**
Test fail = bolt complete না। কোনো exception নেই।

---

## 🎯 এরপর কী?

- [specs.md documentation](https://specs.md) — official docs
- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) — AWS official AI-DLC rules
- [fabriqaai/specs.md](https://github.com/fabriqaai/specs.md) — specs.md framework

---

## 🙏 শেষ কথা

এই repo-টা লেখা হয়েছে এই বিশ্বাসে যে — ভালো documentation পড়তে bore লাগা উচিত না।

Rafiq ভাই, Sami, Nila — এই তিনজন কাল্পনিক চরিত্র। কিন্তু গল্পটা real। প্রতিটি project-এ এই তিনজন থাকে। AI-DLC-এর কাজ তাদের একই page-এ রাখা।

Happy building! 🚀

---

[← পর্ব ৫](./পর্ব-৫-operations-phase.md) | [শুরুতে ফিরে যাও →](./README.md)
