# 🤦 PM বললেন, Developer কাঁদলেন

> *"Feature টা simple। শুধু একটু user registration।"*
> — PM, ইতিহাসের প্রতিটি project-এ

তারপর কী হলো? ৩ সপ্তাহ পরে backend developer আবিষ্কার করলেন frontend developer সম্পূর্ণ ভিন্ন একটা flow বানিয়েছেন। আর PM বললেন — *"আরে এটা তো আমি বলিনি!"*

চেনা চেনা লাগছে? 😅

এই repo-টা সেই গল্পের একটা সুখী সংস্করণ — যেখানে AI-DLC ব্যবহার করে **PM ভাই**, **BE ভাই** (Backend), আর **FE আপা** (Frontend) একসাথে বসে requirement থেকে delivery পর্যন্ত পুরো জিনিসটা গুছিয়ে করেছেন। কেউ কাঁদেননি। (প্রায়।)

---

## 🤔 AI-DLC আসলে কী?

সহজ কথায়: **AI-DLC (AI-Driven Development Lifecycle)** হলো AWS-এর তৈরি একটা methodology যেখানে AI শুধু code লেখার tool না — পুরো software development lifecycle-এর প্রতিটা ধাপে AI একজন active participant।

Traditional approach-এ কী হয়?

```
PM requirement দেন
    ↓
Developer মাথা চুলকান
    ↓
কিছু একটা বানান
    ↓
PM বলেন "এটা না"
    ↓
😭
```

AI-DLC-তে কী হয়?

```
PM intent বলেন
    ↓
AI প্রশ্ন করে, plan বানায়
    ↓
Human approve করেন
    ↓
AI code বানায়
    ↓
Human আবার approve করেন
    ↓
Deploy 🚀
```

মূল দর্শন হলো: **AI প্রস্তাব করে, মানুষ সিদ্ধান্ত নেয়।**

Google Maps-এর মতো — তুমি destination ঠিক করো, AI route বলে দেয়, আর তুমি প্রতিটা turn-এ চোখ রাখো।

---

## 🆚 Traditional SDLC vs AI-DLC

| বিষয় | Traditional SDLC | AI-DLC |
|---|---|---|
| Planning unit | Epic → Sprint (২ সপ্তাহ) | Intent → Unit → Bolt (ঘণ্টা/দিন) |
| Requirement | BA লেখেন, dev পড়েন, সবাই আলাদা বোঝেন | AI সবার সামনে বসে clarify করে |
| Code | Developer লেখেন, review হয় | AI লেখে, Human validate করে |
| Context | প্রতি meeting-এ আবার বোঝাতে হয় | `memory-bank/`-এ সব save থাকে |
| Deploy | DevOps engineer ঘুম হারান | Operations Agent handle করে |
| "এটা তো বলিনি!" | প্রতি sprint-এ | Inception-এই ধরা পড়ে |

---

## 📐 তিনটি Phase

AI-DLC-এর পুরো জীবনচক্র তিনটা phase-এ ভাগ। প্রতিটা phase একটা নির্দিষ্ট প্রশ্নের উত্তর দেয়:

```
┌─────────────────────────────────────────────────┐
│  INCEPTION                                       │
│  "কী বানাবো এবং কেন?" (WHAT + WHY)               │
│  — কোনো code নেই এখনো                            │
└──────────────────┬──────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────┐
│  CONSTRUCTION                                    │
│  "কীভাবে বানাবো?" (HOW)                          │
│  — AI design + code লেখে, human real-time দেখে   │
└──────────────────┬──────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────┐
│  OPERATIONS                                      │
│  "চালু করছি এবং দেখছি"                            │
│  — deploy + monitor, accumulated context দিয়ে   │
└─────────────────────────────────────────────────┘
```

একটা গুরুত্বপূর্ণ কথা: AI-DLC **waterfall না**, এটা একটা **loop**। প্রতিটা Bolt-এর জন্য এই তিন phase ঘুরে আসা যায় — ঘণ্টায় বা দিনে, সপ্তাহে না। আর প্রতিটা phase নিজের complexity বুঝে depth ঠিক করে (adaptive) — ছোট কাজে কম ceremony, বড় কাজে বেশি।

### 🔵 Inception — "কী বানাবো এবং কেন?"

এখানে business intent কে detailed requirement-এ রূপান্তর করা হয়। AI প্রশ্ন করে, মানুষ উত্তর দেয় — এই ritual-টার নাম **Mob Elaboration** (পুরো team একসাথে বসে AI-এর প্রশ্ন ও proposal validate করে)। চারটা ধাপ:

```
Capture (Intent ধরা) → Elaborate (প্রশ্ন করে ambiguity দূর করা)
   → Decompose (Units + Stories-এ ভাঙা) → Plan Bolts (execution order)
```

এখানে একটাও code লেখা হয় না। এখানে প্রশ্ন না করলে পরেই কাঁদতে হবে। **Output:** Intent, Units, Stories (acceptance criteria সহ), আর Bolt Plan।

### 🟢 Construction — "কীভাবে বানাবো?"

Inception-এর validated context নিয়ে AI এবার logical architecture, domain model, code আর test propose করে। এই ritual-টার নাম **Mob Construction** (team real-time-এ technical ও architectural সিদ্ধান্তে clarification দেয়)। প্রতিটা Bolt এই DDD stages-এ execute হয়:

```
Domain Model → Technical Design → ADR (Architecture Decision Record)
   → Code → Tests → Quality Gate
```

প্রতিটা stage-এ human checkpoint। **Output:** Domain Model, Code, Tests — সব quality gate pass করে।

### 🟡 Operations — "চালু করছি এবং দেখছি"

আগের দুই phase-এর accumulated context নিয়ে AI infrastructure-as-code আর deployment manage করে, মানুষ oversight দেয়। চারটা ধাপ:

```
Build → Deploy → Verify (smoke test) → Monitor
```

Production-এ যাওয়ার আগে অবশ্যই human approval। **Output:** Deployment + Monitoring setup।

---

## 📖 পর্বসমূহ

| পর্ব | বিষয় |
|---|---|
| [পর্ব ১ — specs.md ও memory-bank](./পর্ব-১-specsmd-এবং-memory-bank.md) | specs.md কী, install কীভাবে, folder structure কী |
| [পর্ব ২ — চার Agent, চার ভূমিকা](./পর্ব-২-agents-পরিচয়.md) | কে কী করে, কীভাবে invoke করে |
| [পর্ব ৩ — Inception Phase](./পর্ব-৩-inception-phase.md) | PM requirement বলছেন, AI plan বানাচ্ছে |
| [পর্ব ৪ — Construction Phase](./পর্ব-৪-construction-phase.md) | AI code লিখছে, human দেখছে |
| [পর্ব ৫ — Operations Phase](./পর্ব-৫-operations-phase.md) | Deploy, monitor, রাতে ঘুমানো |
| [পর্ব ৬ — পুরো Flow একনজরে](./পর্ব-৬-পুরো-flow.md) | সব একসাথে, checklist সহ |

---

> 💡 **এই repo-তে যা দেখবেন:** একটা real feature — User Registration System (Django + React) — AI-DLC follow করে requirement থেকে delivery পর্যন্ত। তিনজন মানুষ: **PM ভাই**, **BE ভাই** (Backend), আর **FE আপা** (Frontend)। আর একটা AI যে কাজ করে কিন্তু কখনো একা সিদ্ধান্ত নেয় না।
>
> নাম মনে রাখার দরকার নেই — কে কোন role-এ সেটাই যথেষ্ট: **PM ভাই** requirement বলেন, **BE ভাই** backend দেখেন, **FE আপা** frontend দেখেন।

---

[শুরু করুন → পর্ব ১](./পর্ব-১-specsmd-এবং-memory-bank.md)
