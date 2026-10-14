# What it takes to finish this: Outpero clone

Companion to `recon.md`. Written 2026-10-14.

The recon says what to build. This says what has to be true for it to ship, in
what order, and which parts only you can do.

---

## 0. The thing that decides the project

**The critical path is not code. It is telephony and compliance, and it is
sequential, paperwork-gated, and takes weeks.**

If you build for two months and then start DLT registration, you have a demo
that cannot legally place a call. Compliance starts in **week 1**, in parallel
with everything else.

Three hard regulatory facts, verified against 2026 sources:

1. **You cannot dial Indian phones from a generic VoIP stack.** Domestic
   VoIP-to-PSTN dial-out is prohibited in India. PSTN access requires a
   DoT-licensed operator or a Unified Licence VNO (Exotel operates as one).
   You *must* sit on top of a licensed provider. There is no DIY path.
2. **You cannot call from a normal 10-digit number.** Commercial outbound must
   originate from **140-series** (promotional/marketing) or **1600/160-series**
   (transactional/service). Using the wrong series is itself a violation —
   reported exposure runs to ₹1,50,000 per call in the worst band.
3. **If you run calls for other businesses, you are a Registered Telemarketer.**
   The DLT chain is three parties: your customer is the **Principal Entity**
   (the brand on the call), **you** are the Telemarketer, and your carrier is
   the Aggregator. All three register separately, and the PE↔TM linkage must be
   active or the calls are unauthorised. **This shapes your product**: customer
   onboarding has to collect and verify their PE registration and link it to
   you. Outpero's "DLT-registered" badge is this, productised.

## 1. Only you can do these — start now

I cannot do any of this. Every item is weeks of lead time.

| # | Item | Needs | Lead time |
| --- | --- | --- | --- |
| 1 | Registered company (Pvt Ltd or LLP) | — | 1–3 weeks |
| 2 | GST registration + PAN + bank account | company | 1–2 weeks |
| 3 | **DLT Principal Entity registration** | CoI, GST, PAN, signatory | 2–5 working days, ₹5,900–7,500 |
| 4 | **Registered Telemarketer (RTM) registration** | the above | ~₹5,000 + GST + security deposit |
| 5 | Header / caller-ID approval | PE ID | 24–72 h per header |
| 6 | Call template registration per flow | PE ID | 24–72 h each |
| 7 | **Carrier/CPaaS contract** with SIP + realtime media streaming | KYC | 1–4 weeks |
| 8 | 140 and/or 1600 series numbers | DLT + carrier | days after carrier |
| 9 | NCPR/DND scrubbing access | RTM | with carrier |
| 10 | Payment gateway (Razorpay/Cashfree) | company, GST | 1–2 weeks |
| 11 | Legal review of T&C, privacy, DPDPA posture | counsel | ongoing |

Items 3–9 gate your first real call. Nothing in the build matters until they land.

## 2. Decisions that block architecture

Answer these before `/replica-architect` writes anything.

1. **Which carrier.** The single highest-stakes technical choice. It must
   support **realtime bidirectional media streaming** (WebSocket/SIP media), or
   you cannot build a conversational agent on it at all — you get IVR. Public
   comparisons flag Plivo as cheap (≈₹0.60/min India SIP) but with no documented
   WebSocket audio streaming path; Exotel offers vSIP/AgentStream aimed at voice
   AI with India data residency and DLT hand-holding; FreJun Teler markets
   itself as AI-voice-agent infrastructure (₹600/channel/month + ₹0.15/min).
   **Verify streaming support in a paid trial before signing anything.**
2. **Promotional or transactional.** 140 vs 1600 changes your number series,
   whether DND scrubbing applies, and your answer rates (public figures: 8–20%
   on 140 vs 45–65% on 1600). Lead-callback for a form the user submitted is
   arguably service; cold outreach is promotional. Get this in writing.
3. **Voice stack.** Orchestration (LiveKit Agents / Pipecat / Vapi), ASR, LLM,
   TTS. For Telugu, Indian vendors (e.g. Sarvam) are the realistic path on both
   quality and cost; Western TTS at premium tiers will not fit ₹3.5/min.
4. **Build vs buy the agent runtime.** Vapi/Retell get you to a demo fast but
   own your margin and do not handle Indian compliance. Self-orchestrated is
   slower but is the actual business.
5. **Scope of language.** Telugu+English only (like their pricing page says) or
   the "10+ languages" the marketing claims. Pick one and be honest.

## 3. Verify the unit economics before you build

Do this as a one-week spike. It can kill the project cheaply.

Outpero's headline is **₹3.5/min**. Your cost per minute is roughly:

```
telephony          ₹0.60 – ₹1.80   (aggregator vs direct SIP)
ASR                ?               (streaming, Telugu)
LLM                ?               (streaming, low latency)
TTS                ?               (streaming, Telugu, natural)
infra/recording    ?
-------------------------------------------------
must land under ~₹2.50 to sell at ₹3.5 with any margin
```

The two `?` rows that matter are Telugu TTS and ASR. **Price them for real, at
streaming latency, before committing.** If your all-in cost is ₹4/min, the
business does not exist at their price and you must reposition up-market.

Also note the number rental: they charge ₹649/month per line and bill minutes
separately. That is a sensible model — the line has a fixed carrier cost.

## 4. Build order

Compliance (§1) runs in parallel through all of this.

**Phase 0 — spike, 1–2 weeks.** One hard-coded script, one number, one voice.
Prove: a real Telugu call connects, latency under ~1.5s, barge-in works,
code-switching holds. **Nothing else matters until this is true.** If this fails,
stop and rescope.

**Phase 1 — vertical slice (F02), 3–4 weeks.** Webhook lead in → dial in <30s →
converse → extract fields → summary + transcript → dashboard row. One flow, end
to end, ugly but real.

**Phase 2 — the product shell, 3–4 weeks.** Auth, agent CRUD, script editor,
voice picker, test call, call log, call detail. This is the M-sized dashboard
half and the part I can move fastest on.

**Phase 3 — compliance engine, 2–3 weeks.** Not optional, not deferrable:
DND scrub **at dial time, not campaign start** (the registry updates daily);
9am–9pm window enforced in the dialer; frequency caps (currently 3/day, 8/week
promotional); opt-out captured mid-call and written back; AI disclosure line at
call start; audit logs for every one of these. Recording retention 6 months
general, 60 months for BFSI.

**Phase 4 — billing, 1–2 weeks.** Credit wallet, per-second metering rounded to
30s, ring time and unanswered calls not billed, per-call credit display, low
balance warning, graceful goodbye at zero.

**Phase 5 — campaigns + integrations, 3–4 weeks.** CSV upload, concurrency,
retries, calling window; Sheets, Calendar, WhatsApp (official BSP), webhooks,
CRM.

**Phase 6 — harden, 2–3 weeks.** `/replica-test` on all flows, load at target
concurrency, failure paths (jitter, mid-call hangup, provider outage).

**Phase 7 — brand, launch, deploy.** `/replica-brand` sweep must come back clean
before `/replica-deploy` will ship.

Roughly **16–22 weeks** to a defensible v1 with one person building full time.
Matches the recon's L sizing. Phase 0 failing is the main schedule risk.

## 5. The quality bar nobody budgets for

A voice agent demo is easy. A voice agent that survives 500 calls a day is not.
Budget explicitly for:

- **Silence and turn-taking.** Dead air kills calls faster than a wrong answer.
- **Barge-in.** Indians interrupt on phone calls constantly.
- **Tenglish.** Mid-sentence code-switching in *recognition*, not just speech.
- **Names and numbers.** Indian names, amounts, addresses, "double five".
- **Noise.** Traffic, shops, bad signal, speakerphone.
- **Failure behaviour.** What it says when it does not understand, when the
  caller is angry, when it must hand to a human.

Build an eval set of 50–100 real recorded calls early and score every change
against it. Without that you are tuning on vibes.

## 6. Honest market read

Outpero's own `/compare` page names six competitors (Vapi, Retell, Bland, Bolna,
Zudu, OmniDimension). Searching the Indian market surfaced roughly ten more
actively marketing in 2026: SquadStack, Caller.digital, Vyora, Tabbly, Rootle,
VoiceLink, FreJun, Awaaz, plus Exotel AgentStream and Ozonetel moving in from
telephony.

**This is a crowded market with well-funded incumbents.** "An AI voice agent for
Indian businesses" is not a differentiator in 2026. You need a wedge:

- **One vertical, done properly.** Real-estate site visits, or diagnostics OPD
  booking, or COD confirmation — with the integrations and the script library
  that vertical actually needs.
- **One language pair, genuinely better.** Being clearly the best at Telugu beats
  being tenth-best at ten languages.
- **Compliance as the product.** Most vendors bolt it on. PE-linkage onboarding,
  dial-time scrubbing and audit-ready logs as first-class features is a real
  enterprise wedge.
- **One published honest price.** Outpero currently shows five different price
  points across its own pages (see `recon.md`). Transparency is a cheap,
  immediate differentiator.

Pick one. Run `/replica-entrepreneur` on the competitor set before you commit.

## 7. Division of labour

**I can do:** architecture and schema, the entire web app, the agent runtime
and integrations, the compliance engine, billing/metering, tests, landing page,
store listing, deploy config, and the rebrand sweep.

**You must do:** company and GST, DLT PE + RTM registration, headers and
templates, the carrier contract and numbers, DND access, payment gateway, legal
sign-off, the vertical choice, and the judgement call on whether the Telugu
voice quality is good enough to sell. That last one needs a native ear.

## 8. Do these three things this week

1. **Start DLT and company registration.** Longest lead time, blocks everything.
2. **Open paid trials with two carriers** and confirm in writing that they
   support realtime media streaming for AI agents, and which number series they
   will give you. Test from real SIMs on Jio, Airtel and Vi.
3. **Price Telugu ASR and TTS at streaming latency** and do the §3 arithmetic.

Then come back and we run `/replica-architect`.
