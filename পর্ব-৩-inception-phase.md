# পর্ব ৩ — Inception Phase 🎯

> *"Requirement simple। User register করবে। ব্যস।"*
> — PM (সকাল ১০টায়)
>
> *"তো... ৩টা flow, ৩টা role, dual approval loop, আর feedback cycle?"*
> — Backend Developer (বিকেল ৪টায়)

Inception Phase-এর কাজই হলো সকালের "simple requirement" থেকে বিকেলের "এত কিছু?!" পর্যন্ত পুরো রাস্তাটা **code লেখার আগেই** আবিষ্কার করা।

---

## 🎬 দৃশ্যকল্প

**রুমে তিনজন:**
- 🧑‍💼 **Rafiq ভাই** — PM
- 👨‍💻 **Sami** — Backend Developer (Django)
- 👩‍💻 **Nila** — Frontend Developer (React)

**লক্ষ্য:** User Registration System বানাতে হবে।

---

## ধাপ ১ — Master Agent দিয়ে Project Initialize

Sami প্রথমে terminal খোলে:

```
/specsmd-master-agent project-init
```

**Master Agent জিজ্ঞেস করে:**

```
Master: Tech stack কী হবে?
```

**Sami বলে:**
```
Backend: Django + Django REST Framework
Frontend: React
Database: PostgreSQL
Testing: pytest-django (backend), Jest + RTL (frontend)
```

**Master Agent জিজ্ঞেস করে:**
```
Master: Coding conventions?
```

**Sami বলে:**
```
Backend: snake_case, RESTful API, JSON responses
Frontend: camelCase, functional components
```

**Master Agent file তৈরি করে:**

```
memory-bank/standards/tech-stack.md        ✅ তৈরি
memory-bank/standards/coding-standards.md  ✅ তৈরি
memory-bank/standards/system-architecture.md ✅ তৈরি
```

`tech-stack.md` দেখতে এরকম:

```markdown
# Tech Stack

## Backend
- Framework: Django + Django REST Framework
- Language: Python 3.11+
- Database: PostgreSQL
- Auth: Django built-in + JWT
- Email: Django email backend

## Frontend
- Framework: React 18
- Language: JavaScript/TypeScript
- Routing: React Router v6

## Testing
- Backend: pytest-django
- Frontend: Jest + React Testing Library
```

⛔ **Human Checkpoint:** "এই standards কি ঠিক আছে?"

Sami ও Nila দেখেন, Sami বলেন "হ্যাঁ।"

---

## ধাপ ২ — Inception Agent: Intent Capture

এখন Rafiq ভাই সামনে বসেন। Sami command দেয়:

```
/specsmd-inception-agent intent-create
```

**Inception Agent:**
```
Inception: আপনার business goal কী? কী বানাতে চান?
```

**Rafiq ভাই বলেন:**
```
আমাদের system-এ তিনটা role আছে — Super Admin,
Company Admin, আর Company Employee।

User register করতে পারবে তিনভাবে:
১. Super Admin invite করবে
২. Company Admin invite করবে — Super Admin approval লাগবে
৩. User নিজে apply করবে — Company Admin তারপর Super Admin approve করবে

আর Company Employee-কে Super Admin চাইলে
Company Admin বানিয়ে দিতে পারবে।
```

**Inception Agent intent file তৈরি করে:**

```
memory-bank/intents/001-user-registration/requirements.md  ✅
memory-bank/intents/001-user-registration/system-context.md ✅
```

`requirements.md`-এর শুরুটা এরকম:

```markdown
# Intent: Multi-Role User Registration System

## Statement
একটি multi-role user registration system যেখানে
Super Admin, Company Admin, এবং Company Employee
তিনটি আলাদা flow-এ register করতে পারবে।

## Roles
- super_admin
- company_admin
- company_employee
```

---

## ধাপ ৩ — Mob Elaboration: AI প্রশ্ন করে, সবাই মিলে উত্তর দেয়

এটাই Inception Phase-এর সবচেয়ে গুরুত্বপূর্ণ অংশ। Inception Agent একে একে সব ambiguity তুলে ধরে।

**Inception Agent:**
```
প্রশ্ন ১: Invitation link কতক্ষণ valid থাকবে?
          Expire হলে কী হবে?
```

**Rafiq ভাই:**
```
৭২ ঘণ্টা। Expire হলে নতুন invite পাঠাতে হবে।
```

**Inception Agent:**
```
প্রশ্ন ২: Flow 2-তে Super Admin feedback দিলে
          Company Admin update করে পাঠায়।
          এই loop কি unlimited? নাকি limit আছে?
```

**Rafiq ভাই একটু ভাবেন:**
```
Hmm... unlimited রাখি এখন।
```

**Inception Agent:**
```
প্রশ্ন ৩: Flow 3-তে Super Admin feedback দিলে
          user আবার form submit করে।
          সেটা কি Company Admin থেকে আবার শুরু হবে?
```

**Rafiq ভাই:**
```
হ্যাঁ, পুরো loop restart।
```

**Inception Agent:**
```
প্রশ্ন ৪: একই email দিয়ে একাধিক pending
          invitation থাকতে পারবে?
```

**Sami:**
```
না, same email + same company-তে একটাই
active invitation থাকতে পারবে।
```

**Inception Agent:**
```
প্রশ্ন ৫: Registration complete হওয়ার পর
          password setup link কতক্ষণ valid?
```

**Rafiq ভাই:**
```
২৪ ঘণ্টা।
```

সব উত্তর `requirements.md`-এ যোগ হয়:

```markdown
## Clarifications

- Invitation link expiry: 72 ঘণ্টা
- Feedback loop: unlimited iterations
- Flow 3 SA feedback: restarts from Company Admin
- Duplicate email+company: blocked
- Password setup link: 24 ঘণ্টা
```

⛔ **Human Checkpoint:** "সব clarification কি correct?"

তিনজনই দেখেন, approve করেন।

---

## ধাপ ৪ — Unit Decomposition

Inception Agent এখন requirements পড়ে feature-টাকে ভেঙে Units বানায়:

**Inception Agent propose করে:**

```
Proposed Units:

Unit 1: Role & Permission Model (Backend)
Unit 2: Invitation Core Engine (Backend)
        — Token generation, status machine, email
Unit 3: Flow 1 — Super Admin Direct Invite
Unit 4: Flow 2 — Company Admin Invite + Approval Loop
Unit 5: Flow 3 — Self Registration + Dual Approval
Unit 6: Shared Invitation Details Page (Frontend)
Unit 7: Password Setup & Activation
Unit 8: Role Promotion

Dependency Map:
Unit 1 → Unit 2 → Unit 3, 4, 5 → Unit 6, 7
Unit 1 → Unit 8 (independent)

✋ এই breakdown কি ঠিক আছে?
```

**Sami:** "Unit 2 আর Unit 4-এর approval API আলাদা করাটা ঠিক আছে।"
**Nila:** "Unit 6 shared — এটা important। ঠিক আছে।"

**✅ Approved।**

Inception Agent file তৈরি করে:

```
memory-bank/intents/001-user-registration/units/
    ├── unit-1-role-model/
    │   ├── unit-brief.md     ✅
    │   └── stories/
    ├── unit-2-invitation-engine/
    │   ├── unit-brief.md     ✅
    │   └── stories/
    ├── unit-3-flow1/
    │   ├── unit-brief.md     ✅
    │   └── stories/
    ... (বাকিগুলোও)
```

`unit-2-invitation-engine/unit-brief.md`:

```markdown
# Unit 2: Invitation Core Engine

## Purpose
Invitation তৈরি, token generation, status management,
এবং email dispatch সব এই unit handle করবে।

## Depends On
Unit 1 (Role Model)

## Key Concepts
- Invitation token (UUID, 72h expiry)
- Status machine (8টি status)
- Email templates
```

---

## ধাপ ৫ — Stories লেখা

প্রতিটি Unit-এর জন্য Inception Agent stories লেখে। Unit 2-এর একটা উদাহরণ:

**Inception Agent:**

```
Unit 2-এর জন্য Stories propose করছি:

Story 2.1: Invitation Token Generation
"As the system, I want to generate a unique expiring
token so that only the intended recipient can act."

Acceptance Criteria:
- Token: UUID-based, cryptographically secure
- Expiry: 72 hours (configurable)
- Expired token → "Link expired" page
- Duplicate email+company → block

Story 2.2: Invitation Status Machine
"As the system, I want to track invitation status
at every step."

Status transitions:
pending_user_approval → registered (Flow 1)
pending_user_approval → pending_super_admin (Flow 2)
pending_company_admin → pending_super_admin / company_feedback
company_feedback → pending_user_approval
pending_super_admin → registered / super_admin_feedback
super_admin_feedback → [flow-dependent]

✋ Stories কি সব business rules cover করেছে?
```

**Rafiq ভাই:** "Status machine-এ 'rejected' আর 'expired' status যোগ করো।"

**Inception Agent:** "অবশ্যই।" — stories update করে।

Stories file তৈরি হয়:

```
memory-bank/intents/001-user-registration/units/
    unit-2-invitation-engine/stories/
        ├── US-201-token-generation.md   ✅
        ├── US-202-status-machine.md     ✅
        └── US-203-email-dispatch.md     ✅
```

---

## ধাপ ৬ — Bolt Planning

```
/specsmd-inception-agent bolt-plan
```

Inception Agent সব units + stories দেখে Bolt plan তৈরি করে:

**Inception Agent:**

```
Proposed Bolt Plan:

Unit 1:
  Bolt 1.1: Django User Model Extension
  Bolt 1.2: Role-based Permission Setup

Unit 2:
  Bolt 2.1: Invitation Model & Status Machine
  Bolt 2.2: Email Service
  Bolt 2.3: Token Validation API

Unit 3:
  Bolt 3.1: Flow 1 Backend API
  Bolt 3.2: Flow 1 Frontend UI

Unit 4:
  Bolt 4.1: Company Admin Invite API
  Bolt 4.2: Super Admin Review API
  Bolt 4.3: Company Admin Resubmit API
  Bolt 4.4: Flow 2 Frontend

Unit 5:
  Bolt 5.1: Self Registration API
  Bolt 5.2: Company Admin Review API
  Bolt 5.3: Super Admin Review API (Flow 3)
  Bolt 5.4: Flow 3 Frontend

Unit 6:
  Bolt 6.1: Shared Invitation Details Page
  Bolt 6.2: Expired/Invalid Token Page

Unit 7:
  Bolt 7.1: Password Setup API
  Bolt 7.2: Password Setup UI

Unit 8:
  Bolt 8.1: Role Promotion API
  Bolt 8.2: Role Promotion UI

Total: 18 Bolts
Execution order: Unit 1 → Unit 2 → Unit 3,4,5 (parallel possible)
                 → Unit 6,7 → Unit 8

✋ Bolt plan approve করবেন?
```

**Sami:** "Unit 3, 4, 5 parallel করতে পারব — Bolt 2.1 শেষ হলে।"
**Nila:** "Unit 6, 7 আমি শুরু করব Unit 2.3 শেষ হলে।"

**✅ Approved।**

Bolt files তৈরি হয়:

```
memory-bank/bolts/
    ├── bolt-101-user-model.md          ← status: planned
    ├── bolt-102-permissions.md         ← status: planned
    ├── bolt-201-invitation-model.md    ← status: planned
    ├── bolt-202-email-service.md       ← status: planned
    ... (বাকি ১৩টি)
```

`bolt-201-invitation-model.md` দেখতে এরকম:

```markdown
---
id: bolt-201
title: Invitation Model & Status Machine
unit: unit-2-invitation-engine
status: planned
stories:
  - US-201
  - US-202
depends_on:
  - bolt-101
  - bolt-102
---

## Summary
Invitation Django model তৈরি করা, 8-state status
machine implement করা, token generation এবং
expiry logic।
```

---

## ⛔ Final Inception Approval Gate

**Inception Agent:**

```
Inception Phase summary:

✅ Intent captured
✅ 6 clarifications resolved
✅ 8 Units defined
✅ 24 Stories written
✅ 18 Bolts planned
✅ Dependency map ready

সব কিছু align আছে।
Construction phase শুরু করব?
```

**Rafiq ভাই, Sami, Nila:** "হ্যাঁ।"

**Master Agent:** "Inception complete। Unit 1 construction শুরু করো।"

---

## 📁 Inception শেষে memory-bank-এর অবস্থা

```
memory-bank/
├── standards/
│   ├── tech-stack.md              ✅
│   ├── coding-standards.md        ✅
│   └── system-architecture.md     ✅
│
└── intents/
    └── 001-user-registration/
        ├── requirements.md        ✅ (stories + NFR + clarifications)
        ├── system-context.md      ✅ (scope, out-of-scope)
        └── units/
            ├── unit-1-role-model/
            │   ├── unit-brief.md  ✅
            │   └── stories/       ✅ (2 stories)
            ├── unit-2-invitation-engine/
            │   ├── unit-brief.md  ✅
            │   └── stories/       ✅ (3 stories)
            ... (বাকি 6 units)

memory-bank/bolts/
    ├── bolt-101.md  ✅ planned
    ├── bolt-102.md  ✅ planned
    ... (18টি bolt, সব planned)
```

এখন একটাও code লেখা হয়নি। কিন্তু সবাই জানেন কী হবে, কীভাবে হবে, কোন order-এ হবে। PM জানেন। Backend জানেন। Frontend জানেন।

এটাই Inception Phase-এর সাফল্য।

---

[← পর্ব ২](./পর্ব-২-agents-পরিচয়.md) | [পর্ব ৪ → Construction Phase](./পর্ব-৪-construction-phase.md)
