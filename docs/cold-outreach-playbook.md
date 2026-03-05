# PropChat — Cold Outreach Playbook (Internal)

> **⚠️ INTERNAL DOCUMENT — Do not publish on landing page. Use as premium content for onboarded agents.**

---

## The Problem

Agents have cold lists from property developers, telco data, or new housing projects. They want to blast these contacts. Direct cold blasting violates WhatsApp policy and PDPA.

## The Solution: Opt-In First Flow

### For Buyer/Tenant Prospecting

```
Step 1: Agent uploads cold list (e.g., 500 contacts from area database)

Step 2: Send a SINGLE opt-in message:
        "Hi [Name], I'm [Agent] from [Agency]. 
         I have exclusive property listings in [Area] — 
         new launches, subsales, and rentals.
         Reply YES to receive updates. Reply STOP to opt out."

Step 3: Only contacts who reply YES become active contacts

Step 4: Broadcast freely to opted-in contacts
```

### For Listing Acquisition (Seller/Landlord Prospecting)

```
Step 1: Agent gets cold list (e.g., new development owner contacts)

Step 2: Send opt-in message:
        "Hi [Name], I'm [Agent] from [Agency]. 
         I specialize in properties around [Area/Development]. 
         If you're considering selling or renting, I'd love to 
         help — free valuation included.
         Reply YES and I'll share more. Reply STOP to opt out."

Step 3: Only contacts who reply YES become active contacts

Step 4: Follow up with valuation offer → listing agreement
```

## Why This Is Legally Safe

| Layer | Status | Why |
|---|---|---|
| **WhatsApp Policy** | ✅ Compliant | First message is 1-to-1, not template blast. Only opted-in contacts receive broadcasts |
| **PDPA Malaysia** | ✅ Compliant | Consent obtained before marketing. STOP option provided |
| **PropChat liability** | ✅ Protected | ToS Section 3 & 7 place data responsibility on agent. Consent is logged |

## Key Rules

1. **First message must be a SINGLE message, not a template broadcast** — use WhatsApp's "service" category, not "marketing"
2. **Always include a STOP option** — PDPA requires opt-out mechanism
3. **Log all YES/STOP responses** — audit trail for compliance
4. **Rate limit opt-in messages** — max 50–100 opt-in requests per day to avoid quality score drop
5. **Never re-message contacts who replied STOP** — this is a PDPA violation

## Block Rate Benchmarks

| Use Case | Expected Block Rate | Quality Impact |
|---|---|---|
| Buyer prospecting (cold) | 15–25% | Manageable if throttled |
| Listing acquisition (cold) | 20–35% | Higher — throttle more aggressively |
| Opted-in contacts (warm) | 2–5% | No impact |

## Recommended Daily Limits for Cold Opt-In

| Account Age | Max Opt-In Requests/Day |
|---|---|
| New account (< 30 days) | 50 |
| Established (30–90 days) | 100 |
| Mature (90+ days, high quality) | 200 |

## Future PropChat Feature: Built-In Opt-In Flow

Auto-handle the opt-in process:
1. Agent uploads cold CSV
2. PropChat sends opt-in request (throttled, compliant)
3. YES replies auto-added to CRM with consent timestamp
4. STOP replies auto-blacklisted
5. Full audit log for PDPA compliance

**This becomes a premium Pro feature — the reason agents pay RM 199/mo instead of using blaster tools.**
