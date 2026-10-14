# Recon map: Outpero (web app + telephony)

Scope: the **Instant Lead Calling** loop, end to end. Agent builder, dedicated
number, outbound call, mid-call actions, post-call summary and delivery.
Bulk Campaign second. Inbound third.
For: a sellable Indian-market AI voice-agent product.
Date: 2026-10-14

> **What this is.** Outpero is *not* a telecalling app in the usual sense — there
> is no human agent, no softphone, no dialer for a seat of callers. It is an **AI
> voice-agent platform**: you configure an "AI employee" (name, voice, phone
> number, script) and it places and answers real phone calls by itself, in
> Telugu/Hindi/English, qualifies the lead, books the appointment and writes its
> own notes. The dashboard is the easy half. The product is the realtime voice
> pipeline and Indian telephony compliance.

## Confidence and method

Public sources only. `app.outpero.com` is behind a login and I did **not** go past
it, did not create an account, and did not read the app's bundles or network
calls. Everything below the sign-in screen is therefore **inferred** from
marketing renders, the pricing page, FAQ schema and the parent company's product
page. Confidence is marked per item. Before `/replica-architect`, the cheapest
way to raise confidence is for you to sign up for the free tier yourself and
click through with me.

- **Seen directly:** the marketing site, the pricing page, the sign-in screen.
- **Described in detail by the vendor:** dashboard tiles, call log, call detail,
  campaign behaviour, mid-call actions, credit system.
- **Guessed:** exact routes, layout specifics, settings/team screens.

## Sources

| # | source | URL | notes |
| --- | --- | --- | --- |
| 1 | marketing home | https://www.outpero.com/ | core pitch, dashboard render, credit model, FAQ |
| 2 | pricing | https://www.outpero.com/pricing | ₹1899/30d + ₹3/min, three voice tiers, billing rules |
| 3 | AI voice agents | https://www.outpero.com/ai-voice-agents | latency/SLA numbers, language scope, CRM behaviour |
| 4 | AI employee | https://www.outpero.com/ai-employee | ₹0 base + ₹3.5/min model, number rental ₹649/mo |
| 5 | deploy | https://www.outpero.com/deploy | CSV bulk, DNC, retries, concurrency, deployment packages |
| 6 | compare | https://www.outpero.com/compare | competitor set: Vapi, Retell, Bland, Bolna, Zudu, OmniDimension |
| 7 | Telugu agents | https://www.outpero.com/telugu-ai-voice-agents | ₹75,000 setup + ₹6/min agency framing, transfer-to-human |
| 8 | fastest employee | https://www.outpero.com/fastest-employee | positioning, city list, "employee not software" framing |
| 9 | T&C | https://www.outpero.com/ai-voice-agents-terms-and-conditions | service scope list |
| 10 | app sign-in | https://app.outpero.com/ | **only** screen seen; Google OAuth + email/password |
| 11 | parent company | https://www.opendg.org/products/ | the three modes, mid-call actions, 30 concurrent, transcripts/sentiment |

## Core loop

**A lead arrives, and within 30 seconds an AI voice employee has called them in
their own language, qualified them, booked the next step and written the notes
into your CRM — without anyone touching a phone.**

Speed is the entire value proposition. Everything else is in service of it.

## Contradictions in public sources (verify before you price against them)

Genuinely useful competitive intel, and the clearest opening for positioning.
Five different public price points for the same product:

| source | model |
| --- | --- |
| home, /ai-employee, /deploy | ₹0 base + from **₹3.5/min** + number **₹649/mo** |
| /pricing | **₹1899** hiring fee /30 days + from **₹3/min** |
| /telugu-ai-voice-agents | **₹75,000** setup + **₹6/min** (drops to ₹5 at volume) |
| /deploy (JSON-LD) | Instant Lead **₹39,999**, Inbound **₹29,999**, Bulk **₹34,999**, ₹999 slot |
| opendg.org | "plans start at **₹4,999/month**" |

Also inconsistent: **concurrency** (20 / 30 / 100+ depending on page);
**inbound** ("coming soon" on home, sold as live on /deploy and opendg);
**languages** ("10+ native languages" on home vs "Telugu+English or English only"
on /pricing); **speed** (<30s on home vs "under 3 seconds" on the Telugu page).
A clone that publishes *one* honest number has an immediate credibility angle.

## Screens

| ID | screen | route / how to reach | purpose | key components | states |
| --- | --- | --- | --- | --- | --- |
| S01 | Sign in | app.outpero.com | auth | Google OAuth, email+password, forgot | default, error, loading |
| S02 | Sign up | from S01 | create account, 50 free credits | form, consent | empty, error, success |
| S03 | Forgot password | from S01 | reset | email form | sent, error |
| S04 | Dashboard | / | "your team at a glance" | 4 stat tiles, team list, attention list | zero-state, active |
| S05 | Swara HR builder | /new | NL → configured agent | chat, live preview | asking, building, done |
| S06 | Employees list | /employees | all AI agents | cards: name, role, calls today, status | empty, populated |
| S07 | Employee detail | /employees/:id | config + stats | tabs, script, voice, fields, number | draft, live, paused |
| S08 | Voice picker | in S07 | choose voice + tier | sample player, tier badges | previewing |
| S09 | Script editor | in S07 | prompt + capture fields | editor, variables, field schema | valid, invalid |
| S10 | Test call | in S07 | ring your own phone | number input, call state | idle, ringing, live, ended |
| S11 | Numbers + KYC | /numbers | provision +91 line | KYC upload, DLT status | none, pending, active |
| S12 | Call log | /calls | every call + credits used | table, filters, outcome chips | empty, filtered |
| S13 | Call detail | /calls/:id | the record | player, transcript, summary, fields | processing, ready |
| S14 | Leads/contacts | /leads | caller memory + history | table, contact drawer | empty, populated |
| S15 | Campaign create | /campaigns/new | CSV upload + mapping | uploader, column map, window | mapping, error, ready |
| S16 | Campaign detail | /campaigns/:id | live progress | progress, concurrency, retries | queued, running, paused, done |
| S17 | Lead sources | /sources | Meta/forms/webhook/CRM | source cards, webhook URL | connected, error |
| S18 | Integrations | /integrations | CRM, Pabbly, Sheets, Calendar, WhatsApp | app cards, OAuth | connected, expired |
| S19 | Credit wallet | /billing/credits | top up, 1 credit = ₹1 | balance, top-up, history | healthy, low, empty |
| S20 | Billing | /billing | invoices, usage | invoice table | — |
| S21 | Analytics | /analytics | campaign + usage reporting | charts, export | no data, populated |
| S22 | Settings | /settings | account, team, DNC | forms, member list | — |
| S23 | Legal centre | /legal | terms, privacy, refund | static | — |
| S24 | Inbound config | /inbound | answer incoming (roadmap) | routing, hours | coming soon |

24 screens. S01 seen directly; S04, S12, S13 strongly evidenced by vendor
renders; the rest inferred from capability claims.

## Flows

```
F01 activate: sign up -> Swara HR brief -> voice + script -> number/KYC -> test call -> live
    happy path: vendor claims "live in 5 minutes" / "2 minutes"
    edge: KYC pending blocks going live; no credits; number unavailable

F02 CORE LOOP instant lead: lead lands (Meta/form/webhook/CRM)
    -> queued -> dialled in <30s -> conversation (barge-in, code-switch)
    -> mid-call action (WhatsApp / calendar / transfer / API)
    -> hangup -> transcript + summary + sentiment + extracted fields
    -> pushed to dashboard, Sheets, WhatsApp, CRM
    edge: no answer -> retry schedule; busy -> auto-reschedule (10:00 busy -> 17:00);
          line jitter -> self-heal; credits exhausted mid-call -> graceful goodbye;
          caller asks for a human -> live transfer; DNC number -> never dialled

F03 bulk campaign: upload CSV -> map columns -> pick employee -> set window
    -> dial N concurrent -> DNC screen -> retries -> results + export
    edge: duplicate numbers, calling window closes mid-run, partial failures

F04 inbound (roadmap): call arrives -> answer 24/7 -> handle -> log -> WhatsApp follow-up

F05 funnel demo: type script -> "Test Call" -> AI rings your phone, free, no card

F06 credits: top up wallet -> consumed per connected second -> low balance warning
    billing rule: per second rounded up to 30s blocks; ring time and unanswered NOT billed

F07 review: call log -> detail -> transcript/recording -> correct fields -> export/CRM

F08 memory: known number calls -> recognised -> "welcome back" with prior context
```

10 flows. F02 is the one people pay for — build it as the vertical slice.

## Components

| component | variants | states | used on |
| --- | --- | --- | --- |
| Button | primary, secondary, ghost, danger | default, hover, focus, disabled, loading | all |
| Stat tile | count, currency | zero, loading, value | S04, S21 |
| Employee card | lead caller, receptionist, admissions | live, busy, paused, needs training | S04, S06 |
| Call row | — | connected, no answer, busy, failed, DNC | S12, S16 |
| Audio player | — | idle, playing, scrubbing | S13 |
| Transcript | turn-by-turn, timestamped | AI turn, caller turn, action event | S13 |
| Field chip | extracted value | confident, low confidence, corrected | S13 |
| Sentiment badge | positive, neutral, negative | — | S12, S13 |
| Voice tier badge | Value, Standard, Premium | selected | S08 |
| Credit meter | — | healthy, low, empty | S04, S19 |
| CSV uploader | — | idle, parsing, mapping, error | S15 |
| Live call indicator | — | ringing, connected, ended | S10, S16 |
| Language toggle | Telugu+English, English | — | S07 |
| Integration card | — | connected, disconnected, error | S17, S18 |
| Empty state | — | — | most |

## Inferred data model

```
Org            id, name, plan, dlt_status, created_at
               confidence: high
User           id, org_id, email, google_sub, role
               evidence: S01 (Google + email auth)   confidence: high
AIEmployee     id, org_id, name, role_label, voice_id, tier, language_mode,
               script_id, number_id, status, memory_enabled
               evidence: "Meera / Lead Caller", voice tiers, S04 render
               confidence: high
VoiceProfile   id, label, tier(value|standard|premium), language, sample_url
               confidence: high
PhoneNumber    id, org_id, e164, provider, dlt_registered, kyc_status, monthly_fee
               evidence: "₹649/month", "Riya's number needs KYC"  confidence: high
Script         id, employee_id, prompt, first_line, version
               evidence: "Outpero Script Editor"   confidence: high
CaptureField   id, script_id, key, label, type, required
               evidence: "4 fields captured", Budget/Timeline  confidence: high
Lead           id, org_id, name, phone, source, custom_attrs, last_called_at
               evidence: caller-memory feature, Sheets sync   confidence: high
Call           id, org_id, employee_id, lead_id, direction, started_at,
               answered_at, ended_at, billable_seconds, credits_used,
               outcome, recording_url, attempt_no
               evidence: pricing page "credits-used column per call"
               confidence: high
TranscriptTurn id, call_id, speaker, text, ts_ms, language
               evidence: "full timestamped transcript"   confidence: high
CallSummary    id, call_id, summary, sentiment, talk_ratio, extracted_json
               evidence: opendg products page   confidence: high
CallAction     id, call_id, type(whatsapp|calendar|transfer|api), payload, ts
               evidence: "live in-call actions"   confidence: high
Campaign       id, org_id, employee_id, name, status, concurrency,
               window_start, window_end, retry_policy
               evidence: /deploy   confidence: high
CampaignTarget id, campaign_id, lead_id, state, attempts, next_attempt_at
               confidence: medium
CreditWallet   id, org_id, balance   (1 credit = ₹1, never expires)
CreditTxn      id, org_id, delta, reason, call_id, created_at
               confidence: high
Integration    id, org_id, kind(meta|webhook|sheets|calendar|whatsapp|crm|pabbly),
               config_json, status
               confidence: high
DNCEntry       id, org_id, phone, source    evidence: /deploy   confidence: high
```

Relationships: Org 1-n User / AIEmployee / PhoneNumber / Campaign; AIEmployee 1-1
Script, 1-n Call; Lead 1-n Call; Call 1-1 CallSummary, 1-n TranscriptTurn,
1-n CallAction; Campaign 1-n CampaignTarget.

~19 entities.

## Out of scope — cannot or should not be cloned

These are `skip` rows in `features.csv`, not backlog items:

- **DLT / telemarketer registration.** TRAI-regulated. You register your own
  entity and headers; it is paperwork and eligibility, not code.
- **+91 number issuance.** Requires a licensed Indian carrier or CPaaS
  (Exotel, Plivo, Knowlarity, Ozonetel, Twilio India). You rent, you don't build.
- **DNC / NCPR scrubbing.** You must screen against the regulated registry via a
  licensed provider. Rebuild the *enforcement UI*, never the registry.
- **The "exclusive" Premium voice library.** Licensed or proprietary TTS voices.
  Use your own vendor and your own voice set.
- **Their Telugu/Tenglish tuning.** The actual moat. You can reach comparable
  quality with your own vendor and your own tuning data — you cannot take theirs.
- **99.9% uptime SLA.** A commercial promise backed by their infrastructure
  spend, not a feature to implement.
- **Their customers, recordings, lead data and call history.**
- **Name, logo, copy, the "Swara"/"Meera" persona names, screenshots.**
  `/replica-brand` sweeps for these before `/replica-deploy` will ship.

## Size

Screens 24, flows 10, entities ~19.

**Hard parts, in order:**

1. **Realtime duplex voice under ~1.4s** — ASR + LLM + TTS streaming with
   barge-in (caller interrupts), endpointing, and no dead air. This is the product.
2. **Telugu⇄English code-switching mid-sentence** in both recognition and speech.
   Most vendors are genuinely bad at this; it is why Outpero exists.
3. **Indian telephony + compliance** — carrier integration, DLT headers, DNC,
   KYC. Weeks of paperwork running parallel to the code.
4. **Per-second metering and billing** that users trust, with the "ring time not
   billed" and "graceful goodbye on zero balance" rules.
5. **Mid-call function calling** — calendar availability, WhatsApp, warm transfer,
   custom API, all without stalling the conversation.
6. **Concurrency** — tens of simultaneous calls with per-org limits.

**Size: XL as a whole — rescope.** The full platform with three modes, your own
tuned voices and national compliance is not a project you finish.

**Size of the recommended slice: L (a quarter)** — Instant Lead Calling only,
one language pair, one voice tier, vendor-assembled voice pipeline
(e.g. LiveKit/Pipecat orchestration, Sarvam or Deepgram ASR, a hosted LLM,
Sarvam/ElevenLabs/Cartesia TTS, Exotel/Plivo for the +91 line). The dashboard
half of that is **M (a few weeks)**; the voice half is where the quarter goes.

**Honest framing:** the UI is a few weeks of solid work. Shipping a voice agent
that holds a natural Telugu call without awkward silence is the real project, and
the gap between a demo that works once and a product that works on 500 calls a
day is most of the quarter.

## Next step

`/replica-architect` — pick the voice stack and the carrier, write the schema,
and order the build so F02 is a thin vertical slice first. Before that, the two
highest-value things you can do: sign up for the free tier and click through it
with me, and run `/replica-entrepreneur` on the competitor set they name on
their own /compare page (Vapi, Retell, Bland, Bolna, Zudu, OmniDimension).
