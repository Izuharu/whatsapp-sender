# PropChat — Project Brief

> **Version:** 1.0 — March 2026
> **Author:** The PropChat Team
> **Status:** Validation Phase (Landing Page + Dashboard Mockup Live)

---

## 1. Core Identity

**Product:** PropChat — WhatsApp CRM for Malaysian Property Agents
**Tagline:** "Never Lose a Lead Again"
**Domain:** TBD (propchat.my / propchat.app)
**Repo:** [github.com/Izuharu/whatsapp-sender](https://github.com/Izuharu/whatsapp-sender)

**The Problem:**
KL property agents use illegal "blaster" tools → get banned. They manage leads in spreadsheets → leads go cold. They copy-paste messages to 200 contacts → 3 hours wasted.

**The Solution:**
A WhatsApp-native CRM that lets agents broadcast listings, auto-follow-up, schedule viewings, and acquire listings — all through the Official WhatsApp Business API. Zero ban risk.

**The Moat:**
The Cold Outreach Playbook — a compliant opt-in flow that teaches agents how to safely prospect cold leads. This is the "secret sauce" competitors don't offer. See `docs/cold-outreach-playbook.md`.

---

## 2. Target Audience

| Segment | Profile | Pain Point |
|---|---|---|
| **Primary** | Solo property agents in KL (REN/REA) | Banned numbers, lost leads, manual work |
| **Secondary** | Small agencies (2-10 agents) | No team coordination, no analytics |
| **Tertiary** | Property developers | Need to reach owners in new developments |

**Market Size (KL):** ~30,000 registered property negotiators (LPPEH Board)

---

## 3. Revenue Model

### Platform Fees (Recurring)

| Plan | Price | Target |
|---|---|---|
| Starter | RM 99/mo | Solo agents starting out |
| Pro Agent | RM 199/mo | Serious agents who scale |
| Agency | RM 399/mo | Teams up to 10 seats |

### Message Credits (Prepaid, Pass-Through + Margin)

| Pack | Price | Per Msg | Your Cost (Meta) | Margin |
|---|---|---|---|---|
| 100 msgs | RM 45 | RM 0.45 | RM 37 | RM 8 (21%) |
| 500 msgs | RM 200 | RM 0.40 | RM 185 | RM 15 (8%) |
| 1,000 msgs | RM 370 | RM 0.37 | RM 370 | RM 0 (at-cost) |
| 5,000 msgs | RM 1,750 | RM 0.35 | RM 1,850 | -RM 100 (loss leader) |

**Note:** 1,000-pack is "Best Value" at-cost to drive volume. Revenue comes from platform fees + smaller credit packs. 5,000-pack is a loss leader for agencies — offset by RM 399/mo agency fee.

### Founder's Batch

- 50 spots, first month of Pro free
- Message credits still purchased separately
- Auto-renews at RM 199/mo after 30 days

---

## 4. Product Features

### Core CRM
- Contact management with tags (Hot/Warm/Cold)
- Filter by interest (Buyer/Seller/Tenant), area, budget
- Import via CSV, manual add, QR code capture

### Broadcasting
- Template-based broadcasts (Meta-approved templates)
- Audience segmentation by tag, area, property type
- Delivery tracking: sent, delivered, read, replied

### Automations
- Drip sequences: Day 1 → Day 3 → Day 7 follow-ups
- Viewing reminders: 24hr + 1hr before
- Cold lead re-engagement after 30 days
- Welcome sequence for new opt-ins

### Listings Management
- Property cards with photos, specs, pricing
- Link listings to broadcast campaigns
- Track inquiries per listing

### Viewing Scheduler
- Calendar view with day/time slots
- Auto-confirmation and reminder messages
- Status tracking: Confirmed, Pending, Cancelled

### Analytics
- Message volume, response rate, reply time
- Quality Score monitoring (Meta's quality rating)
- Top performing areas / listing types
- Contact growth over time

### Opt-In Flow (The Differentiator)
- Upload cold CSV → auto-send opt-in request (throttled)
- YES replies auto-added to CRM with consent timestamp
- STOP replies auto-blacklisted
- Full audit log for PDPA compliance
- Daily limit controls to protect quality score

---

## 5. Technical Stack

### Hosting & Compute

| Component | Service | Why | Escape Plan |
|---|---|---|---|
| **Frontend** | Vercel | Free tier, auto-deploy from Git, edge CDN | Static HTML — deploy anywhere (Cloudflare Pages, Netlify) |
| **Framework** | Next.js (App Router) | SSR for dashboard, API routes for backend | Standard React — portable to any Node host |
| **Database** | Supabase (PostgreSQL) | Free tier, REST API, RLS, real-time | Standard PostgreSQL — migrate to any PG host (Neon, RDS, self-hosted) |
| **Auth** | Supabase Auth or Clerk | Session management, magic links | JWT-based — swap provider without schema changes |
| **File Storage** | Supabase Storage or Cloudflare R2 | Property images, CSV uploads | S3-compatible API — portable to any S3 provider |

### WhatsApp Integration

| Component | Service | Why |
|---|---|---|
| **API Access** | Meta Cloud API (direct) | Free hosting, no BSP middleman fees |
| **Webhook Receiver** | Vercel API Route / Edge Function | Receives message status updates, incoming messages |
| **Template Management** | Meta Business Manager | Create/submit message templates for approval |
| **Fallback BSP** | 360dialog or Gupshup | If direct Meta API becomes limiting at scale |

### Email & Notifications

| Component | Service | Why |
|---|---|---|
| **Transactional Email** | Resend | Free tier (3,000/mo), simple API |
| **Lifecycle Emails** | Resend | Welcome, billing alerts, limit warnings |

### Payments

| Component | Service | Why |
|---|---|---|
| **Subscriptions** | Stripe | MYR support, subscription billing, invoice |
| **Credit Top-ups** | Stripe (one-time payments) | Prepaid credit packs |
| **Webhook** | Vercel API Route | Payment confirmation → credit balance update |

### Background Jobs

| Task | Method |
|---|---|
| Broadcast queue processing | Vercel Serverless + Supabase queue table |
| Drip sequence scheduler | Vercel Cron (free tier: 1/day, Pro: every 1min) |
| Monthly credit resets | Cron job gated by CRON_SECRET header |
| Quality score check | Daily cron → Meta Graph API |

---

## 6. Database Schema (Core Tables)

```sql
-- Users / Agents
users (id, email, phone, name, agency, plan, credits_balance, created_at)

-- Contacts
contacts (id, user_id, name, phone, email, interest, budget, area, tag, consent_status, consent_date, created_at)

-- Broadcasts
broadcasts (id, user_id, template_id, name, audience_filter, status, sent_count, delivered_count, read_count, reply_count, scheduled_at, sent_at)

-- Messages
messages (id, user_id, contact_id, broadcast_id, direction, content, status, wa_message_id, created_at)

-- Listings
listings (id, user_id, title, type, area, price, bedrooms, sqft, level, image_url, inquiry_count, status, created_at)

-- Viewings
viewings (id, user_id, contact_id, listing_id, date, time, status, reminder_sent, created_at)

-- Automations
automations (id, user_id, name, trigger, sequence_json, is_active, created_at)

-- Credit Transactions
credit_transactions (id, user_id, type, amount, balance_after, stripe_payment_id, created_at)

-- Opt-In Requests (Audit Trail)
opt_in_requests (id, user_id, contact_phone, message_sent, response, responded_at, created_at)
```

---

## 7. Portability / Escape Plan

**Portability Score: 95/100**

| Component | Lock-in Risk | Escape Path |
|---|---|---|
| **Vercel** | Low | Static export + any Node host (Railway, Fly.io, EC2) |
| **Supabase** | Low | Standard PostgreSQL — pg_dump and restore anywhere |
| **Supabase Auth** | Medium | JWT-based — swap to Clerk, Auth0, or custom |
| **Stripe** | Low | Industry standard — no migration needed |
| **Meta Cloud API** | Medium | Switch to BSP (360dialog, Gupshup) if needed |
| **Resend** | Low | Swap to Mailgun, SES, or Postmark (same API pattern) |

### Architecture Rules for Portability
1. **All business logic in `src/lib/`** — decoupled from Vercel/Next.js specifics
2. **Database access through a data layer** — no direct Supabase client calls in components
3. **Environment variables for all service credentials** — never hardcoded
4. **WhatsApp integration behind an adapter pattern** — swap Meta direct for BSP without changing business logic
5. **No vendor-specific features dependency** — avoid Vercel KV, Supabase Realtime unless truly needed

---

## 8. Compliance & Legal

| Requirement | Status |
|---|---|
| Privacy Policy | ✅ Live (`privacy.html`) — PDPA-focused |
| Terms of Service | ✅ Live (`terms.html`) — consent clause, indemnification |
| PDPA Data Controller/Processor split | ✅ Documented — PropChat is Processor, Agent is Controller |
| WhatsApp Business Policy compliance | ✅ Opt-in flow designed, cold outreach playbook created |
| Opt-out mechanism | ✅ STOP keyword handling in playbook |
| Data retention policy | ✅ 30-day deletion after account cancellation |

---

## 9. Current Status

### Completed (Validation Phase)
- [x] Landing page with hero, features, pain points, pricing, waitlist
- [x] Interactive dashboard mockup (8 views, property photos)
- [x] Prepaid credit pricing model with transparent per-message rates
- [x] Privacy Policy & Terms of Service
- [x] Supabase waitlist (email + phone collection)
- [x] GitHub repo → Vercel deployment pipeline
- [x] Cold Outreach Playbook (internal)
- [x] Custom SVG favicon/branding

### Next Phase (Development)
- [ ] Set up Next.js project structure
- [ ] Implement auth (Supabase Auth or Clerk)
- [ ] Build core CRM (contacts CRUD)
- [ ] Integrate Meta Cloud API (send/receive messages)
- [ ] Build broadcast engine with queue
- [ ] Implement credit system with Stripe
- [ ] Build opt-in flow automation
- [ ] Analytics dashboard (real data)
- [ ] Onboarding flow ("Mission Start" — first link in 30 seconds)

---

## 10. Key Metrics to Track

| Metric | Target (Month 3) |
|---|---|
| Waitlist signups | 100+ |
| Paying users | 20+ |
| MRR | RM 3,000+ |
| Message credits sold | RM 5,000+/mo |
| Agent churn rate | < 15%/mo |
| Avg messages/agent/mo | 300+ |

---

> **Agent Mantra:** PropChat is not a blaster tool. It's a WhatsApp CRM that teaches agents the *right way* to prospect, follow up, and close — while keeping their number safe. The playbook is the product.
