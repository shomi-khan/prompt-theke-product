# পর্ব ৫ — Operations Phase 🚀

> *"Code তো হলো। এখন শুধু deploy করলেই হবে।"*
> — যেকোনো developer, disaster-এর ঠিক আগে

Operations Phase মানে শুধু `git push` না। এটা একটা structured process — build করো, staging-এ test করো, human approve করো, তারপর production। আর তারপর চোখ রাখো।

Operations Agent এই পুরোটা handle করে। তুমি শুধু checkpoints-এ থাকো।

---

## ⚙️ Operations Agent-কে ডাকো

Sami command দেয়:

```
/specsmd-operations-agent deploy
```

**Operations Agent:**

```
Operations Agent: memory-bank পড়লাম।
                 Construction complete দেখছি।
                 Deploy process শুরু করছি।

কোথায় deploy করব?
❯ Staging
  Production
  Local Docker
```

**Sami:** "Staging আগে।"

---

## Stage 1: Build

```
Operations Agent: Build শুরু করছি।

✅ Backend Docker image build হলো
   → python:3.11-slim base
   → requirements install
   → collectstatic

✅ Frontend build হলো
   → npm run build
   → bundle size: acceptable

✅ Environment variables চেক:
   DATABASE_URL              ✅
   SECRET_KEY                ✅
   EMAIL_HOST                ✅
   EMAIL_PORT                ✅
   EMAIL_HOST_USER           ✅
   INVITATION_EXPIRY_HOURS   ✅ (72)
   PASSWORD_RESET_EXPIRY     ✅ (24)

✅ Security scan:
   → কোনো hardcoded secret নেই
   → dependency vulnerability নেই

⛔ Build artifacts ready।
   Staging deploy করব?
```

**Sami:** "হ্যাঁ।"

---

## Stage 2: Staging Deploy

```
Operations Agent: Staging deploy করছি।

✅ Docker containers up

✅ Database migrations run করলাম:
   → 0001_create_company_user
   → 0002_create_invitation
   → সব migration clean

✅ Smoke tests:

   Flow 1 test:
   → SA invite পাঠালাম test@email.com-এ
   → Email গেলো ✅
   → Invitation link click করলাম ✅
   → Details page দেখাচ্ছে ✅
   → Approve করলাম ✅
   → Password setup email গেলো ✅
   → Password set করলাম ✅
   → Login হলো ✅

   Flow 2 test:
   → CA invite → user approve → SA feedback
   → CA resubmit → user approve → SA approve
   → Password set → Login ✅

   Flow 3 test:
   → Self reg → CA feedback → user resubmit
   → CA approve → SA approve → Login ✅

   Edge cases:
   → Expired link → correct error page ✅
   → Duplicate invite → blocked ✅
   → Role promotion → works ✅

⛔ Staging সব ঠিক আছে।
   Rafiq ভাইকে staging দেখাবেন?
```

**Sami:** "হ্যাঁ, Rafiq ভাইকে ডাকি।"

Rafiq ভাই staging-এ নিজে তিনটা flow test করেন।

**Rafiq ভাই:** "একদম ঠিক আছে। Production-এ যাও।"

---

## Stage 3: Production Deploy

```
Operations Agent: Production deploy-এর আগে checklist:

✅ Database backup নেওয়া হয়েছে
✅ Rollback plan ready আছে
✅ Off-peak time (রাত ১১টা)
✅ On-call team জানানো হয়েছে

⛔ এটা production। একবার deploy হলে
   user-রা ব্যবহার শুরু করবে।

   FINAL APPROVAL: Production deploy করব?
```

**Sami:** "হ্যাঁ, করো।"

```
Operations Agent: Blue/Green deployment শুরু:

→ নতুন version (Ranking System) deploy হলো
→ পুরনো version standby-তে আছে (24h)

Traffic shift:
→ 10% traffic → 15 মিনিট monitor করলাম → ✅
→ 50% traffic → 15 মিনিট monitor করলাম → ✅
→ 100% traffic → ✅

Production deploy সম্পন্ন 🎉

memory-bank/operations/deployment-context.md আপডেট করলাম।
```

---

## Stage 4: Monitor

```
Operations Agent: Monitoring setup করছি।

Alert rules তৈরি হলো:
  ⚠️ Invitation email delivery failure > 1%
  ⚠️ Token validation error rate > 0.5%
  ⚠️ Password setup failure > 1%
  ⚠️ DB write latency > 200ms
  ⚠️ 500 error rate > 0.1%

Dashboard তৈরি হলো:
  📊 Daily invitations sent (flow type অনুযায়ী)
  📊 Approval funnel: invited → approved → registered
  📊 Average time: invite → registration complete
  📊 Bounce rate on invitation details page

48 ঘণ্টার watch period শুরু হলো।
```

**48 ঘণ্টা পরে:**

```
Operations Agent: 48h watch period complete।

Report:
  ✅ Flow 1: 23 invitations → 19 registered
  ✅ Flow 2: 8 invitations → 6 registered (2 pending)
  ✅ Flow 3: 4 self-registrations → 3 registered
  ✅ Email delivery rate: 99.2%
  ✅ Zero 500 errors
  ✅ Zero security incidents

⛔ Feature stable এবং healthy।
   Final sign-off দিবেন?
```

**Rafiq ভাই:** "Perfect। Sign-off দিলাম।"

---

## 🎊 শেষ কথা

Rafiq ভাই সেই কথাটা বললেন না — *"এটা তো আমি বলিনি।"*

কারণ Inception Phase-এই সব বলা হয়েছিল, সব প্রশ্ন করা হয়েছিল, সব document হয়েছিল।

Sami কাঁদলেন না।
Nila frustrated হলেন না।
PM খুশি।

**AI-DLC-এর সাফল্য এখানেই।**

---

[← পর্ব ৪](./পর্ব-৪-construction-phase.md) | [পর্ব ৬ → পুরো Flow একনজরে](./পর্ব-৬-পুরো-flow.md)
