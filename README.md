# gohighlevel whatsapp chatbot: how to set one up that reads context, handles the 24-hour window, and books on your calendar

The reason "GoHighLevel WhatsApp chatbot" is such an annoying search is that GHL already has WhatsApp, and it already has an AI. Stack them together and you get something that answers. Whether it *books* is a different question.

The usual failure looks like this: a coach or clinic has 100+ open WhatsApp threads in the GoHighLevel Conversations inbox, one VA typing generic follow-ups that ignore what the lead actually said, and a 24-hour messaging window that quietly closes on half of them. GHL's workflow automations handle the template part fine. They just don't read a conversation and reply like a person who was listening.

That's the gap an AI agent is supposed to fill. This piece walks through what a WhatsApp chatbot on GHL actually has to handle, the two realistic routes (GHL's built-in Conversation AI and a marketplace app layered on top), and what it costs — including the WhatsApp-specific details that don't show up until something silently fails to send.

## WhatsApp inside GHL is not SMS with a different icon

Everything else in this article depends on understanding one billing rule and one timing rule.

GoHighLevel bills WhatsApp per delivered message rather than charging a flat monthly fee, and replies are governed by WhatsApp's own conversation rules. When a customer messages you, a **24-hour customer service window** opens. Inside that window you can send free-form messages. Once it closes, you can only re-open the thread with a marketing or utility template that Meta has approved in advance.

Two numbers worth knowing, straight from GoHighLevel's documentation:

- Each WhatsApp Business Account gets **1,000 free service conversations per month**, and free-form replies inside an open window don't cost a conversation.
- A **Click-to-WhatsApp ad or Facebook CTA button** opens a longer, 72-hour entry-point window instead of the standard 24.

So a useful WhatsApp agent has to do more than generate good copy. It has to know whether the window is open, whether a template is required, and which thread is already being handled by a human. GHL exposes a "WhatsApp: Customer Service Window Check" condition for workflows precisely because that state matters. If your AI setup ignores it, you get messages that either fail silently or fall outside the compliant path.

## Route 1: HighLevel's own Conversation AI

GHL's Conversation AI bot setup wizard lists WhatsApp among the supported channels, alongside SMS, email, Facebook, Instagram and the chat widget. If your only requirement is "something replies on WhatsApp," this is the cheapest place to start — HighLevel's pricing guide lists an AI Employee unlimited plan at $97/month per enabled location covering Conversation AI usage.

Where it tends to run out is the conversation itself. Its agents are built around large prompts rather than discrete, checkable steps, which makes it hard to guarantee that every lead gets asked the same qualifying questions in the same order, or to branch differently for a tenant versus a landlord. GHL's own feature request board reflects the rest of the gap: there's an open item about sending images and voice notes through to an AI agent, with the note that WhatsApp audio and image processing exists inside GHL's interface but isn't exposed through the API.

That's not a knock on the native tool. It's the reason a third-party app market exists on top of GHL in the first place.

## Route 2: a sub-account app that takes over WhatsApp in your CRM

This is the route most agencies running high WhatsApp volume end up on, and CloseBot is the app that comes up most often in that conversation — it's the one that appears in GHL's marketplace and connects to your sub-account through a standard OAuth approval.

The architectural point matters more than the feature list: CloseBot doesn't connect to WhatsApp itself. It connects to your CRM and takes over the text-based channels already flowing through it. Your WhatsApp Business number lives in HighLevel; CloseBot answers what arrives in that inbox. A SetSmart review of the tool makes this explicit — CloseBot's integrations are HighLevel, HubSpot, LeadConnector and custom CRMs, and "the WhatsApp Business number is provisioned and billed through your CRM or a provider, and CloseBot rides on top of it."

What that gets you in practice:

- **Channel filters.** You can point one agent at WhatsApp only, and leave SMS or the chat widget to someone else. Filters work on tags too, so you can require a tag like `whatsapp-inbound` and exclude `ai off` to stop the agent replying on threads your team has taken over.
- **Objective-driven flows instead of one giant prompt.** Qualification becomes a sequence of steps with checkpoints, which is what makes the "if the workshop is in three weeks, the goal is to invite to the workshop" style of rule actually implementable.
- **Conversational booking on GHL calendars**, including reschedules and cancellations. A SetSmart review notes CloseBot retries a booking when a calendar throws an error instead of falling back to a "sorry, that slot is taken" message.
- **Personas** that control tone, message splitting and even occasional typos, so replies don't arrive as one perfect wall of text.
- **A testing portal, human takeover, and Smart FAQ** — when the agent hits a question it can't answer, it flags it rather than inventing one; you answer once and it re-engages every lead who asked.
- **Provider fallback** across five model providers, so one provider's outage doesn't take your WhatsApp replies down with it.
- **Knowledge base auto-refresh.** Scraped sites are re-checked daily and updated automatically, rather than requiring a manual re-scrape every time a price changes.

One honest limitation: the agent only knows what's in GHL. CloseBot's own team has said on Reddit that if a message isn't in your GHL conversation, the agent won't know about it — and separately that WhatsApp is a large channel for their users outside the US, which is where the audio and image handling actually matters.

Ready to test it against your own WhatsApp threads? 👉 [Start free on your GoHighLevel sub-account](https://app.closebot.com/a?fpr=li87) — 100 replies a month, no card.

## The two WhatsApp edge cases that decide whether this works

### Voice notes and images

This is the single most-asked question about WhatsApp AI on GHL, and the answer is genuinely split. CloseBot ships image analysis — their changelog lists "AI view and analyze images" as released in July 2025, aimed at exactly the businesses that get photos of a floor plan, a tattoo reference or a broken part. Audio messages are the murkier half: GHL's feature request board carries an open item titled "Send Images and Voice notes to closebot," with the explanation that CloseBot is designed to handle both, but GHL's current API instability gets in the way.

If your leads send a lot of voice notes — common in Latin America, the Middle East and parts of Europe — test this specifically during your trial with real voice notes before you commit, rather than assuming it works because the specs imply it should.

### Message billing isn't one-for-one with replies

CloseBot bills per segment: one message equals one segment. The exception is the Agent Node with "unlimited potential" switched on — many tools, unlimited instruction size — where billing moves to token costs and a single reply can consume several segments. If you're building a heavy agent with multiple tools, budget for more than the message count suggests.

## Setting it up: the actual sequence

1. **Enable WhatsApp on the GHL location.** A WhatsApp number has to be subscribed and active on the sub-account before anything else works, and any template you plan to send outside the 24-hour window needs Meta approval first. Do this early — approvals aren't instant.
2. **Connect the sub-account to CloseBot.** In CloseBot, go to Sources, add a new source, pick HighLevel Sub-Account, and approve the OAuth prompt. You select the specific GHL sub-account during that flow.
3. **Build a job flow and filter it to WhatsApp.** Set the channel filter to WhatsApp only, then add tag filters so the agent stays out of conversations your team already owns.
4. **Write the persona before the objectives.** Tone, sentence length, how messages get broken up.
5. **Upload knowledge.** Business info, pricing, FAQs, service areas — and the website scrape if your services change often.
6. **Add the booking node and connect the GHL calendar.** Qualification steps first, then the calendar, so disqualified leads never reach the slot picker.
7. **Test in the testing portal, then go live.** Keep a pause mechanism available so any single conversation can be handed back to a human mid-thread.

The realistic timeline for a working agent on one niche is a day, not a project. The thing that takes longer is deciding what "qualified" means for your business.

## What it costs

CloseBot's plans page carries a June 2026 last-modified date. Here's what it shows.

| Plan | Who it's for | What's included | Price | Billing |
| --- | --- | --- | --- | --- |
| Free | Testing, or under 100 replies a month | 1 agent, 1 user seat, 1 MB storage, unlimited account connections, 100 monthly messages | $0 | Always free, no card |
| Core — Business | Businesses automating their own pipeline | Message costs included in the base price, 15+ templates (50+ on annual), human support, extra users at $5/seat, add-on storage and agents, WhatsApp available through your GHL channels | from $64/mo | Monthly, or $53/mo equivalent billed as $640/yr |
| Core — Agency | Agencies building and re-billing agents for clients | Unlimited agents and sources, white-label client portal, client wallets, re-bill all costs including seats and storage, rebillable per-message cost | $397/mo | Monthly |
| Growth | Teams needing SLAs, compliance or high volume | 50+ templates, HIPAA compliance, quarterly audits, 99.99% priority uptime, priority support | Custom | Talk to sales |

Three usage details that change the maths more than the headline price:

- **Messages.** Free plan: 100 included, then $0.08 per message pay-as-you-go. Business plans: a 500-message ceiling is included, and raising the ceiling lowers the per-message rate; overage draws from a wallet at a 2x rate. Agency accounts are billed per message at a flat, rebillable rate — the plans page currently lists **$0.012 per message**, though older CloseBot documentation still shows $0.006, so confirm the current figure with sales if that spread matters to your margin.
- **Storage.** Business plans include 1 MB of knowledge storage, with add-ons from $0.10 to $3.00 per MB per month. Agency accounts pay $0.006 per MB per day, rebillable. For scale reference: 1 MB of text is roughly 1,000 pages.
- **Seats.** One user is included; additional users are $5 each on both business and agency plans, and agencies can mark that up.

Two things the page states plainly and you should factor in: **there are no refunds**, and every paid plan comes with a **7-day trial** — so the trial is where you do your testing, not after. There's also no bring-your-own-API-key option; CloseBot frames that as a security decision, which means your model spend is baked into the plan rather than billed separately.

There's one official discount: the code **CLOSEBOT100OFF** takes $100 off your first payment, applied at checkout or under Settings → Subscription. Public partner codes circulate elsewhere, but CloseBot's own coupon page says it only maintains that one — and no code unlocks features you wouldn't otherwise get.

Worth keeping in perspective: CloseBot is one line of the bill. Your GoHighLevel subscription sits underneath it, and Meta's per-conversation WhatsApp charges sit under that. If you're an agency, the rebilling model is what turns those layers from cost into margin. 👉 [See the current plan breakdown and start a 7-day trial](https://app.closebot.com/a?fpr=li87).

## Business plan or agency plan?

If you're using the agent on your own pipeline, the business plan is the one that makes sense — message costs are included in the base price, which keeps the bill predictable as WhatsApp volume fluctuates.

The agency plan only pays for itself if you're reselling. It unlocks the white-labeled client portal, client wallets funded by Stripe into your account, and markup control on messages, seats and storage. CloseBot's own polled figure is that agencies bill an average of $500 per client per month. At $397 flat plus a rebillable per-message cost, that spread is the business case, and it's the reason the plan exists.

One caveat if you're agency-side and planning to include a WhatsApp agent in a productized offer: the 24-hour window is a client-facing operational reality you'll have to explain, because it's the reason you can't simply have the AI chase cold WhatsApp leads with free-form messages at will.

## What users and reviewers actually say

CloseBot's G2 review summary describes reviewers consistently praising ease of use and quick setup, and highlighting conversation automation and lead management as the strongest parts. On r/automation, one user's verdict was blunt: "way better than GHL chat AI — you can conversationally book appointments and reschedule."

The most useful thread for WhatsApp specifically is on r/gohighlevel, where a consultant described a coaching client with 100+ open GHL WhatsApp conversations and a VA sending generic follow-ups. CloseBot's team replied there that many of their users run WhatsApp this way, and that the agent reads the full conversation displayed in GHL — with the caveat above about messages that never made it into the CRM.

The counterweight comes from SetSmart's review, which rates the conversation quality highly but flags the architecture: CloseBot is the brain and your CRM is the nervous system, so you're paying for two products if you don't already run one. If you're already on GHL — which is the premise of this entire article — that objection mostly disappears.

## Quick answers

**Does CloseBot connect to WhatsApp directly?** No. It connects to your CRM, and WhatsApp has to be enabled on your GoHighLevel location. Your CRM is where the number lives and where the message charges land.

**Can the agent reply outside the 24-hour window?** Free-form replies only work while the window is open. Outside it, you need an approved marketing or utility template in GHL. Plan your follow-up sequences around that, not against it.

**Can it handle voice notes?** Test it. CloseBot supports image analysis today; WhatsApp audio depends on what GHL's API passes through, and that gap has been an open request on GHL's own board.

**Is there a free way to try this?** Yes — the free plan allows 100 replies a month with one agent and unlimited account connections, and every paid plan carries a 7-day trial.

**What happens if the agent doesn't know an answer?** Smart FAQ flags the question instead of improvising, you answer once, and the agent re-engages every lead who asked.

If your WhatsApp threads are already in GoHighLevel, the setup work is a day and the free plan is enough to prove whether the agent books better than your VA does. If WhatsApp isn't wired into your GHL location yet, sort that out first — no agent can answer a channel that isn't connected.
