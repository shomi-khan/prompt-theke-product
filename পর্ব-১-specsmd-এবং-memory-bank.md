# পর্ব ১ — specs.md ও memory-bank 🗂️

> *"আরেকটা tool? সত্যিই?"*
> — প্রতিটি developer, নতুন কিছু দেখলে

হ্যাঁ, আরেকটা tool। কিন্তু এটা actually কাজের। promise।

---

## 🔧 specs.md কী?

**specs.md** হলো একটা open-source framework যেটা AI-DLC methodology implement করতে সাহায্য করে। এটা কোনো SaaS না, কোনো subscription না — এটা basically একগুচ্ছ markdown file যেগুলো তোমার AI coding tool (Claude Code, Cursor, Copilot) কে বলে দেয় "এখন তুমি Inception Agent-এর মতো আচরণ করো।"

মানে agent বলতে আলাদা কোনো software না। **Agent = AI tool + সঠিক instructions।**

GitHub: [fabriqaai/specs.md](https://github.com/fabriqaai/specs.md)

specs.md তিনটা flow support করে:

| Flow | কখন ব্যবহার করবে |
|---|---|
| **Simple** | শুধু spec তৈরি করতে চাইলে, execution দরকার নেই |
| **FIRE** | দ্রুত কাজ, brownfield project, কম ceremony চাই |
| **AI-DLC** | পুরো methodology, DDD সহ, complex project |

আমরা **AI-DLC** flow নিয়ে কাজ করব।

---

## ⚙️ Prerequisites

```bash
node --version
# Node.js 18+ লাগবে

# আর যেকোনো একটা AI coding tool:
# - Claude Code (Anthropic)
# - Cursor
# - GitHub Copilot
```

---

## 📦 Install করব কীভাবে?

Project folder-এ গিয়ে একটাই command:

```bash
npx specsmd@latest install
```

Terminal-এ জিজ্ঞেস করবে:

```
? Select a development flow:
❯ AI-DLC - Full methodology with DDD
  FIRE - Adaptive execution, brownfield ready
  Simple - Spec generation only
```

**AI-DLC** select করো।

তারপর জিজ্ঞেস করবে:

```
? Select your AI coding tool:
❯ Claude Code
  Cursor
  GitHub Copilot
  Other
```

তোমারটা select করো। শেষ।

---

## 📁 .specsmd — এটা কী?

Install হওয়ার পরে project-এ একটা `.specsmd/` folder তৈরি হয়। এটা হলো specs.md-এর **engine room** — এখানে হাত দেওয়ার দরকার নেই, কিন্তু কী আছে জানা ভালো।

```
.specsmd/
├── manifest.yaml              ← কোন flow install হয়েছে, কোন version
└── aidlc/                     ← AI-DLC flow-এর সব কিছু
    ├── agents/                ← চারটি agent-এর instruction files
    │   ├── master-agent.md
    │   ├── inception-agent.md
    │   ├── construction-agent.md
    │   └── operations-agent.md
    ├── skills/                ← Agent-রা কোন কাজে কোন skill use করবে
    ├── templates/             ← requirements.md, unit-brief.md ইত্যাদির template
    └── memory-bank.yaml       ← memory-bank-এর schema definition
```

**সহজ ভাষায়:** `.specsmd/` = agent-দের পাঠ্যপুস্তক। AI এখান থেকে পড়ে শেখে তার কী করতে হবে।

### Slash commands কোথায় install হয়?

AI tool অনুযায়ী আলাদা জায়গায়:

```
Claude Code ব্যবহার করলে:
.claude/commands/
    ├── specsmd-master-agent.md
    ├── specsmd-inception-agent.md
    ├── specsmd-construction-agent.md
    └── specsmd-operations-agent.md

Cursor ব্যবহার করলে:
.cursor/commands/
    └── (একই files)

GitHub Copilot ব্যবহার করলে:
.github/agents/
    └── (একই files .agent.md format-এ)
```

এই files-এর কারণেই `/specsmd-inception-agent` টাইপ করলে AI বোঝে এখন সে Inception Agent।

---

## 🧠 memory-bank — এটা কী?

এটাই সবচেয়ে গুরুত্বপূর্ণ জিনিস।

AI-এর কোনো memory নেই। আজকে যা বললে, কাল নতুন session-এ সব ভুলে যাবে। **memory-bank হলো সেই সমস্যার সমাধান।** এটা project-এর persistent knowledge base — disk-এ থাকা plain markdown files যেগুলো AI প্রতিটি session-এ শুরুতে পড়ে নেয়।

মানে Monday-তে Claude Code-এ কাজ শুরু করলে, Wednesday-তে Cursor-এ switch করলেও কোনো সমস্যা নেই — memory-bank-এর files একই থাকবে।

### memory-bank-এর সম্পূর্ণ structure:

```
memory-bank/
│
├── standards/                 ← Master Agent তৈরি করে (project-init এ)
│   ├── tech-stack.md          ← Django, React, PostgreSQL — কী ব্যবহার হচ্ছে
│   ├── coding-standards.md    ← snake_case, camelCase, কোন style follow হবে
│   └── system-architecture.md ← overall architecture কেমন
│
├── intents/                   ← Inception Agent তৈরি করে
│   └── {intent-id}/           ← যেমন: 001-user-registration/
│       ├── requirements.md    ← Stories + Acceptance Criteria + NFR
│       ├── system-context.md  ← Scope, out-of-scope, constraints
│       └── units/             ← feature-এর বড় বড় টুকরা
│           └── {unit-id}/     ← যেমন: unit-1-role-model/
│               ├── unit-brief.md   ← এই unit কী করবে
│               └── stories/        ← এই unit-এর user stories
│
├── bolts/                     ← Inception + Construction তৈরি করে
│   ├── bolt-001-role-model.md      ← planned/in-progress/completed
│   ├── bolt-002-invitation.md
│   └── ...
│
└── operations/                ← Operations Agent তৈরি করে
    └── deployment-context.md  ← environment, infra, monitoring config
```

### কোন agent কোথায় লেখে?

```
Master Agent      → memory-bank/standards/
Inception Agent   → memory-bank/intents/
                    memory-bank/bolts/ (plan করে)
Construction Agent→ memory-bank/bolts/ (execute করে, status update)
Operations Agent  → memory-bank/operations/
```

---

## 🗃️ পুরো project structure একনজরে

Install হওয়ার পরে তোমার project দেখতে এরকম হবে:

```
my-project/
│
├── .specsmd/              ← specs.md engine (ছুঁবে না)
│   └── aidlc/
│       ├── agents/
│       ├── skills/
│       └── templates/
│
├── .claude/               ← Claude Code-এর slash commands
│   └── commands/
│
├── memory-bank/           ← project-এর brain (এখানেই সব থাকবে)
│   ├── standards/
│   ├── intents/
│   ├── bolts/
│   └── operations/
│
└── src/                   ← তোমার actual code
    ├── backend/
    └── frontend/
```

---

## 💡 একটা গুরুত্বপূর্ণ কথা

> Agent stateless — মানে প্রতিটা `/specsmd-inception-agent` call নতুন করে শুরু হয়। Agent শুরুতে memory-bank পড়ে context নেয়। তাই **প্রতিটা step-এর পরে file save হয়েছে কিনা নিশ্চিত করতে হবে।** Chat-এ বললেই হবে না, disk-এ লিখতে হবে।

---

পরের পর্বে দেখব চারটা agent কে কে, কার কী কাজ, এবং কীভাবে ডাকতে হয়।

---

## 📦 Full & Final Folder Structure (এই পর্ব শেষে)

Install + `project-init` এর পরে, code লেখা শুরুর ঠিক আগে — পুরো project দেখতে এরকম হবে। এটাই এই পর্বের reference structure:

```
my-project/
│
├── .specsmd/                          ← specs.md engine (ছুঁবে না)
│   ├── manifest.yaml                  ← কোন flow, কোন version
│   └── aidlc/
│       ├── agents/                    ← ৪টি agent-এর instruction
│       │   ├── master-agent.md
│       │   ├── inception-agent.md
│       │   ├── construction-agent.md
│       │   └── operations-agent.md
│       ├── skills/                    ← কোন কাজে কোন skill
│       ├── templates/                 ← requirements.md, unit-brief.md ইত্যাদির template
│       └── memory-bank.yaml           ← memory-bank schema
│
├── .claude/                           ← AI tool-এর slash commands
│   └── commands/                      ← (Cursor হলে .cursor/commands/,
│       ├── specsmd-master-agent.md       Copilot হলে .github/agents/)
│       ├── specsmd-inception-agent.md
│       ├── specsmd-construction-agent.md
│       └── specsmd-operations-agent.md
│
├── memory-bank/                       ← project-এর brain (সব context এখানে)
│   ├── standards/                     ← Master Agent লেখে
│   │   ├── tech-stack.md
│   │   ├── coding-standards.md
│   │   └── system-architecture.md
│   ├── intents/                       ← Inception Agent লেখে (এখনো খালি)
│   ├── bolts/                         ← Inception + Construction লেখে (এখনো খালি)
│   └── operations/                    ← Operations Agent লেখে (এখনো খালি)
│
└── src/                               ← তোমার actual code (এখনো খালি)
    ├── backend/
    └── frontend/
```

> 📌 এখন `intents/`, `bolts/`, `operations/`, `src/` খালি — এগুলো পরের phase-গুলোতে ভরবে। কিন্তু কাঠামোটা শুরুতেই জানা থাকলে পরে হারাবে না।

---

[← README](./README.md) | [পর্ব ২ → চার Agent, চার ভূমিকা](./পর্ব-২-agents-পরিচয়.md)
