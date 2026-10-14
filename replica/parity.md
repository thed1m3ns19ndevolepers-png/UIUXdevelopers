## Parity: 2.0 / 100

features 2.0  (65 counted, must-haves 0 of 31 done)

Not shippable yet: 31 must-have features are not done.

## By area, weakest first
- auth                           0.0  (3 features)
- onboarding                     0.0  (2 features)
- agent                          0.0  (8 features)
- voice                          0.0  (4 features)
- core                           0.0  (1 features)
- integrations                   0.0  (6 features)
- reliability                    0.0  (3 features)
- calls                          0.0  (10 features)
- billing                        0.0  (7 features)
- actions                        0.0  (4 features)
- leads                          0.0  (1 features)
- campaigns                      0.0  (6 features)
- inbound                        0.0  (1 features)
- dashboard                      0.0  (3 features)
- analytics                      0.0  (1 features)
- settings                       0.0  (2 features)
- telephony                     25.0  (2 features)
- compliance                    50.0  (1 features)

## Missing, in build order
- [must] agent: Choose language mode for the agent, no  (Telugu+English or English only)
- [must] agent: Create an AI employee with a name and role, no
- [must] agent: Define fields the agent must capture, no  (e.g. budget, timeline)
- [must] agent: Pick a voice from a library, no  (your own voices not theirs)
- [must] agent: Test call to your own phone before going live, no  (the whole top-of-funnel demo)
- [must] agent: Write and edit the call script or prompt, no
- [must] auth: Sign in and session handling, no
- [must] auth: Sign up with email or Google, no
- [must] billing: Bill per connected second rounded to 30s blocks, no
- [must] billing: Credit wallet with top-up, no  (1 credit = ₹1)
- [must] billing: Credits used shown per call, no  (their stated anti-bill-shock feature)
- [must] billing: Do not bill ring time or unanswered calls, no
- [must] calls: AI summary of each call, no
- [must] calls: Call detail view with player and transcript, no
- [must] calls: Call log listing every call, no
- [must] calls: Classify the call outcome, no
- [must] calls: Extract structured fields from the conversation, no
- [must] calls: Record the call, no  (with consent and retention rules)
- [must] calls: Timestamped turn-by-turn transcript, no
- [must] core: Call a new lead within 30 seconds of arrival, no  (speed is the entire value proposition)
- [must] dashboard: Dashboard with headline stats, no  (employees working, conversations, qualified leads, credits spent)
- [must] integrations: Receive leads by webhook, no
- [must] leads: Lead and contact database, no
- [must] reliability: Retry on no answer, no
- [must] settings: Legal pages, no  (terms, privacy, refund)
- [must] telephony: Place an outbound call over a real phone line, no  (via carrier API)
- [must] voice: Barge-in so the caller can interrupt the agent, no
- [must] voice: Code-switching between Telugu and English mid-sentence, no  (hardest single feature)
- [must] voice: Natural duplex conversation with low latency, no  (the actual product; target under ~1.4s)
- [must] compliance: DNC and do-not-call screening, partial  (enforce it in your dialer; the registry itself is regulated)
- [must] telephony: KYC flow for the number, partial  (you build the UI; the verification is the provider's)
- [should] actions: Book a calendar slot during the call, no  (Google Calendar API)
- [should] actions: Send a WhatsApp message during the call, no  (official WhatsApp Business API)
- [should] actions: Transfer the live call to a human, no  (blind and warm)
- [should] agent: Pause and resume an employee, no
- [should] agent: Voice quality tiers at different per-minute rates, no  (Value Standard Premium)
- [should] analytics: Campaign and usage analytics, no
- [should] auth: Forgot and reset password, no
- [should] billing: Invoices and usage history, no
- [should] billing: Low balance warning, no
- [should] calls: Remember a returning caller and their context, no  (the welcome back behaviour)
- [should] calls: Sentiment on each call, no
- [should] campaigns: Dial many numbers concurrently, no  (claims vary: 20, 30, 100+)
- [should] campaigns: Export campaign results, no
- [should] campaigns: Map CSV columns to agent variables, no
- [should] campaigns: Monitor campaign progress live, no
- [should] campaigns: Respect a calling window, no
- [should] campaigns: Upload a CSV for a bulk campaign, no
- [should] dashboard: Per-employee performance on the dashboard, no  (calls today and a status)
- [should] integrations: Push call data to a CRM, no  (Salesforce, HubSpot, Zoho via official APIs)
- [should] integrations: Receive leads from Meta and Instagram lead ads, no  (official Meta API only)
- [should] integrations: Receive leads from a website form, no
- [should] integrations: Sync results to Google Sheets, no
- [should] onboarding: Free credits granted on signup, no  (vendor grants 50 credits)
- [should] onboarding: Guided agent builder from plain language, no  (their "Swara HR" conversational setup)
- [should] reliability: Auto-reschedule on busy, no  (10:00 busy, retried 17:00)
- [should] voice: Multiple Indian languages, no  (home claims 10+; pricing page says Telugu+English only)
- [could] actions: Call a custom API mid-conversation, no
- [could] billing: Graceful goodbye when credits run out mid-call, no  (nice touch worth keeping)
- [could] calls: Talk-ratio metric, no
- [could] dashboard: Needs-attention nudges, no  (hot leads waiting, KYC pending, agent underperforming)
- [could] inbound: Answer inbound calls 24/7, no  (marked coming soon on their home page)
- [could] integrations: Pabbly Connect and Zapier automation, no
- [could] reliability: Recover mid-call from network jitter, no  (their "self-healing" claim)
- [could] settings: Team members and roles, no

## Left out on purpose (not scored)
- Provision a dedicated +91 calling number: licensed carrier or CPaaS; you rent a number you do not issue one
- DLT registration for outbound headers: TRAI-regulated; register your own entity
- 99.9% uptime SLA: a commercial promise backed by infrastructure spend not a feature
- Their tuned Telugu voice models: their moat; use your own vendor and your own tuning
- Their exclusive premium voice library: licensed or proprietary voices
- Their customer call recordings and lead data: their customers' data

## Yours, not in the original (not scored)
- Correct an extracted field by hand
- Published single honest price
