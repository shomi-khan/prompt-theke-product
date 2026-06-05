# পর্ব ৪ — Construction Phase 🔨

> *"এখন বলো AI, code লেখো।"*
> — Sami, optimistically

> *"অবশ্যই। তার আগে domain model approve করুন।
>  তারপর API design approve করুন।
>  তারপর code plan approve করুন।
>  তারপর—"*
> — Construction Agent

> *"...ঠিক আছে।"*
> — Sami, humbled

Construction Phase মানে AI এবং human একসাথে বসে কাজ করা। AI একা দৌড়ায় না, human ঘুমায় না।

---

## 🏗️ Construction Agent কীভাবে কাজ করে?

প্রতিটি Bolt-এর জন্য Construction Agent এই stages follow করে:

```
Stage 1: Domain Model     → business entities কীভাবে সাজাবো?
Stage 2: Technical Design → API, patterns, architecture
Stage 3: Code Plan        → কোন file আগে, কোনটা পরে
Stage 4: Code Generation  → actual code
Stage 5: Quality Gate     → test + lint + typecheck — সব pass করতে হবে
```

প্রতিটি stage-এ ⛔ Human Checkpoint আছে।

---

## 🎬 Bolt 1.1: Django User Model Extension

Sami command দেয়:

```
/specsmd-construction-agent bolt-start
```

Construction Agent memory-bank পড়ে শুরু করে:

```
Construction Agent: Bolt 1.1 শুরু করছি।
                   [bolt-101-user-model.md পড়লাম,
                    tech-stack.md পড়লাম]

Stage 1 — Domain Model:

Django-তে সবার আগে User model extend করতে হবে।
কারণ পরে change করা অনেক কঠিন।

Proposed entities:

Company:
  - id, name, created_at

User (AbstractUser extend):
  - role: super_admin / company_admin / company_employee
  - company: FK → Company (nullable)
  - is_active: False by default
    (password set হলে True হবে)

✋ এই domain model কি ঠিক আছে?
```

**Sami:** "is_active=False default — এটা important। ঠিক আছে।"

```
Construction Agent: Stage 2 — Technical Design:

Migration strategy:
- AbstractUser extend করতে হবে শুরুতেই
- AUTH_USER_MODEL = 'accounts.User' settings-এ
- Company আগে create হবে, User পরে

✋ Approve?
```

**Sami:** "হ্যাঁ।"

```
Construction Agent: Stage 3 — Code Plan:

এই order-এ files তৈরি হবে:
1. accounts/models.py  (Company + User model)
2. accounts/admin.py   (Django admin registration)
3. accounts/migrations/0001_initial.py
4. accounts/tests/test_models.py

✋ Plan approve করবেন?
```

**Sami:** "ঠিক আছে।"

```
Construction Agent: Stage 4 — Code লিখছি...

accounts/models.py তৈরি করলাম।
  → Company model: id, name, created_at
  → Role choices: super_admin, company_admin, company_employee
  → User model: AbstractUser extend,
                role field, company FK,
                is_active=False default

accounts/tests/test_models.py তৈরি করলাম।
  → User creation test
  → Role assignment test
  → Company relation test

Stage 5 — Quality Gate:
```

```
✅ python manage.py makemigrations — success
✅ python manage.py migrate — success
✅ pytest accounts/tests/ — 6 tests passed
✅ flake8 accounts/ — no errors

Bolt 1.1 → COMPLETED ✅
bolt-101-user-model.md → status: completed
```

---

## 🎬 Bolt 2.1: Invitation Model & Status Machine

এটা পুরো system-এর সবচেয়ে critical bolt।

```
Construction Agent: Bolt 2.1 শুরু করছি।
                   [bolt-201, requirements.md, stories পড়লাম]

Stage 1 — Domain Model:

Invitation entity:

id: UUID (primary key)
token: UUID (unique, invitation link-এ যাবে)
flow: super_admin_invite / company_admin_invite / self_registration
status: 8টি possible value (নিচে দেখুন)
email, name: invitee-র info
company: FK → Company
role: invited role
invited_by: FK → User (nullable, Flow 3-এ null)
company_admin_feedback: TextField
super_admin_feedback: TextField
expires_at: DateTimeField (72h after creation)

Status values:
  pending_user_approval
  pending_company_admin
  company_feedback_given
  pending_super_admin
  super_admin_feedback
  registered
  rejected
  expired

✋ Domain model ঠিক আছে?
```

**Sami একটু ভেবে:** "unique_together লাগবে email + company-তে — duplicate block করতে।"

```
Construction Agent: ঠিক বলেছেন। যোগ করছি।
                   unique_together: [email, company, status]
                   (terminal status বাদে)

✋ এখন approve?
```

**Sami:** "হ্যাঁ।"

```
Construction Agent: Stage 2 — Technical Design:

Status Machine rules:

pending_user_approval → registered          (Flow 1 only)
pending_user_approval → pending_super_admin (Flow 2: user approved)
pending_user_approval → rejected

pending_company_admin → pending_super_admin (CA approved)
pending_company_admin → company_feedback_given

company_feedback_given → pending_user_approval (user notified)

pending_super_admin → registered
pending_super_admin → super_admin_feedback

super_admin_feedback →
  Flow 2: company_feedback_given (CA must update)
  Flow 3: pending_user_approval (loop restart)

Invalid transition হলে → Exception raise করবে

✋ State machine logic ঠিক আছে?
```

**Sami:** "Flow 3-তে SA feedback দিলে Company Admin থেকে পুরো loop restart — এটা confirm। ঠিক আছে।"

```
Construction Agent: Stage 3 — Code Plan:

1. invitations/models.py        (Invitation model)
2. invitations/state_machine.py (transition validator)
3. invitations/migrations/
4. invitations/tests/test_state_machine.py
   → সব valid transitions test হবে
   → invalid transition exception test হবে

✋ Plan approve?
```

**Sami:** "হ্যাঁ।"

```
Construction Agent: Code লিখছি...

invitations/models.py:
  → InvitationStatus choices (8 values)
  → InvitationFlow choices (3 values)
  → Invitation model with all fields
  → is_expired() method
  → save() override for expires_at default

invitations/state_machine.py:
  → VALID_TRANSITIONS dict
  → flow-specific transitions (Flow 2 vs Flow 3 different)
  → transition() function with validation
  → InvalidTransitionError custom exception

Quality Gate:
✅ pytest — 12 tests passed
   (সব valid transitions, invalid transition exception,
    duplicate email block, expiry logic)
✅ flake8 — no errors

Bolt 2.1 → COMPLETED ✅
```

---

## 🎬 Bolt 2.2: Email Service

```
Construction Agent: Bolt 2.2 শুরু করছি।

Domain Model: EmailService class
  → send_invitation_to_user()   — invitation link
  → send_feedback_to_company_admin() — SA feedback
  → send_feedback_to_user()     — CA/SA feedback (Flow 3)
  → send_password_setup_email() — registration complete

Technical Design:
  → Django email backend use করব
  → HTML templates (emails/ folder)
  → Template context: name, company, link, feedback text

✋ Approve?
```

**Sami:** "Template-এ expiry time দেখানো দরকার।"

```
Construction Agent: যোগ করছি — expires_at context-এ যাবে।

Code লিখছি...

invitations/email_service.py:
  → InvitationEmailService class
  → 4টি method implement হলো
  → Helper methods: _build_link(), _admin_portal_link()

emails/ templates:
  → invitation_to_user.html
  → feedback_to_company_admin.html
  → feedback_to_user.html
  → password_setup.html

Quality Gate:
✅ pytest — 8 tests passed (email content, link format, expiry shown)
✅ Templates render করছে without errors

Bolt 2.2 → COMPLETED ✅
```

---

## 🎬 Bolt 4.1: Flow 2 — Company Admin Invite API

(Unit 1 ও Unit 2 complete, এখন Flow 2 করা যাবে)

```
Construction Agent: Bolt 4.1 শুরু করছি।

Domain Model:
  CompanyAdminInviteView:
    - Company Admin only access
    - Company auto-set হবে admin-এর company থেকে
    - Role fixed: company_employee

  UserApproveFlow2View:
    - Token validate করবে
    - Status: pending_user_approval → pending_super_admin

  SuperAdminReviewFlow2View:
    - approve → User create + password email
    - feedback → email to Company Admin

  CompanyAdminResubmitView:
    - invitation update করবে
    - আবার user-কে email যাবে

✋ Approve?
```

**Sami:** "SuperAdminReview-এ feedback empty হলে 400 দিতে হবে।"

```
Construction Agent: হ্যাঁ, validation যোগ করছি।

Code Plan:
1. invitations/views.py    (4টি ViewClass)
2. invitations/serializers.py
3. invitations/urls.py
4. invitations/tests/test_flow2.py

Code লিখছি...

[Code generate করল — ViewClass-এ permission checks,
 status transition calls, email dispatch,
 error handling সব আছে]

Quality Gate:
✅ pytest — Flow 2 full loop test pass
   invite → user approve → SA feedback → CA resubmit
   → user approve → SA approve → registered
✅ Empty feedback → 400 ✓
✅ Wrong role access → 403 ✓

Bolt 4.1 → COMPLETED ✅
```

---

## 🎬 Bolt 6.1: Shared Invitation Details Page (Frontend)

এবার Nila-র পালা।

```
Construction Agent: Bolt 6.1 শুরু করছি।

Domain Model (Frontend):
  InvitationDetailsPage component:
    - Token দিয়ে API call করবে
    - Valid → details দেখাবে
    - Expired (410) → LinkExpiredPage
    - Invalid (404) → InvalidLinkPage

States:
  - loading
  - valid invitation (read-only)
  - valid invitation with feedback (feedback banner)
  - Flow 3 resubmit (form editable)
  - expired
  - invalid

✋ State design ঠিক আছে?
```

**Nila:** "Approve/Reject-এর পরে loading state দরকার — double submit prevent করতে।"

```
Construction Agent: ঠিক কথা। isSubmitting state যোগ করছি।

Code Plan:
1. src/pages/InvitationDetailsPage.jsx
2. src/components/InvitationSummary.jsx
3. src/components/FeedbackBanner.jsx
4. src/components/EditableRegistrationForm.jsx
5. src/pages/LinkExpiredPage.jsx
6. src/pages/InvalidLinkPage.jsx
7. tests/InvitationDetailsPage.test.jsx

Code লিখছি...

[React components generate করল — useParams,
 useEffect, conditional rendering,
 isSubmitting state সব আছে]

Quality Gate:
✅ Jest — 14 tests passed
   valid token, expired token, invalid token,
   feedback banner shows, editable form Flow 3,
   double submit prevented
✅ ESLint — no errors

Bolt 6.1 → COMPLETED ✅
```

---

## 📊 Construction শেষে অবস্থা

সব 18টি Bolt complete হওয়ার পর:

```
memory-bank/bolts/
    ├── bolt-101.md  ✅ completed
    ├── bolt-102.md  ✅ completed
    ├── bolt-201.md  ✅ completed
    ├── bolt-202.md  ✅ completed
    ├── bolt-203.md  ✅ completed
    ... (বাকি ১৩টি) ✅ completed
```

**⛔ Construction Final Gate:**

```
Construction Agent: সব 18টি Bolt complete।

Summary:
  ✅ Django User model, roles, permissions
  ✅ Invitation model + 8-state status machine
  ✅ Email service (4 templates)
  ✅ Flow 1: Super Admin direct invite (API + UI)
  ✅ Flow 2: Company Admin invite + approval loop (API + UI)
  ✅ Flow 3: Self registration + dual approval (API + UI)
  ✅ Shared invitation details page
  ✅ Password setup + user activation
  ✅ Role promotion

All quality gates passed।
Operations phase শুরু করব?
```

**Sami, Nila:** "হ্যাঁ।"

---

[← পর্ব ৩](./পর্ব-৩-inception-phase.md) | [পর্ব ৫ → Operations Phase](./পর্ব-৫-operations-phase.md)
