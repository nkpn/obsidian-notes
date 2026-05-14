---
date: 2026-05-14
session_id: local_37227ffc-af52-444b-b6c1-2c1dfc7472f0
type: session
project: shopify-interview-prep
tags: [interview-prep, career, shopify, performance, core-web-vitals, communication]
entities: [[nikita-ilin]], [[speroteck]], [[shopify-plus]], [[hydrogen]], [[liquid]], [[oxygen]], [[checkout-ui-extensions]], [[customer-account-ui-extensions]], [[shopify-functions]], [[bigcommerce-catalyst]], [[core-web-vitals]], [[proj-artifi-configurator]]
tldr: Drafted a senior outstaff "Tell me about yourself" pitch (~80s) anchored in the Speroteck track record (30+ storefronts, 10+ Shopify Plus, 20+ BigCommerce) and a "when not to reach for the new shiny" close. Built standalone answers for parallel-projects, PM communication, the Artifi configurator technical-challenge story (rebuild framing + state-bridge architecture), and a Core Web Vitals (LCP/INP/CLS) deep-dive plus a PDP "loads in parts" diagnostic walkthrough. All scripts grounded in vault facts; every fabricated metric was explicitly flagged as needing verification before interview day.
status: complete
auto-saved: true
---

# Senior Shopify Interview — Self-Introduction & Common Questions

## TL;DR

Long interview-prep session producing ready-to-deliver scripts for the most predictable senior-Shopify questions. The opener leads with shipped volume + solo delivery and closes with the "knowing when *not* to use Hydrogen / Functions / headless" tradeoff line — that's the senior signal Speroteck-style outstaff partners are buying. Standalone answers prepared for parallel projects, PM communication, technical-challenge story (Artifi + productStateManager rebuild), Core Web Vitals targets and tactics, and a PDP-loads-in-parts diagnostic walkthrough. The framing rule throughout: acknowledge the cost / tradeoff before describing the system, never claim numbers you can't defend, and pair every tool name with what you'd look for in it.

## Decisions

- Opener structure is three beats: who I am → what I work in → how I think. Don't rush them together. The third beat (tradeoff thinking, "when not to") is the part that scores senior.
- Drop "I always hit my deadlines" / "I'm a team player" — junior phrases. Replace with concrete behaviors: "I push written status before standup", "no surprises is the rule", "I'd rather flag a risk on day three than apologize on day ten".
- Technical-challenge story leads with outcome ("fully customizable product configurator"), not the stack. Stack names go in sentence two. The "rather than fork the SDK, I wrapped it in my own subscriber layer" line is the senior beat — preserve it.
- Frame the Artifi project as a rebuild, not a greenfield. Use the polite vocabulary (grown organically, accumulated complexity, tightly coupled, outsized risk, architectural reset). Avoid the trash vocabulary (mess, spaghetti, terrible, the old team didn't know what they were doing).
- Never fabricate specific percentages for impact metrics. If real numbers aren't available, stay qualitative ("dropped", "went up noticeably", "effectively went to zero"). Made-up numbers collapse on follow-up.
- For diagnostics, the senior signal is method over tool-naming: form a hypothesis from the symptom, then pick the tool that confirms or kills it. "Loads in parts" is a waterfall signature → look at JS execution timeline first.
- Always pair lab data (Lighthouse, WebPageTest) with field data (CrUX, web-vitals library, PageSpeed Insights field section). Google scores on field data; juniors optimize for Lighthouse alone.
- INP replaced FID in March 2024. Anyone still talking about FID is dating themselves.

## Deliverables

- 60–90s opener script + 25s backup short version, fact-checked against [[nikita-ilin]] and the May 5 Speroteck CV / Shopify prep sessions.
- Standalone answer scripts (~40–50s each) for: "How do you juggle parallel projects?", "How do you work with PMs?", grammar-corrected version of the user's own draft on the same topic.
- Technical-challenge story (~80s) for the Artifi configurator: rebuild context insertion + qualitative impact close.
- Three Core Web Vitals scripts (CSS, JS, images, ~45s each), plus a 60s "How do you think about performance?" master answer.
- PDP "loads in parts" diagnostic walkthrough (~85s) with hypothesis-tree (5 likely causes, ranked) and tool-by-tool breakdown.
- Vocabulary lists — what to use vs. what to avoid — for both the rebuild framing and the multitasking framing.
- Follow-up question banks queued for each major answer (the questions that come next after the user finishes speaking).

## Open threads

- The Artifi configurator project isn't yet in `entities/projects/` — created as a new project entity in this session, but the verification step (what the actual deliverable + measured impact were) is still TBD. Don't claim numbers in interview without a real source.
- The user offered to save the parallel-projects + PM-communication scripts as a follow-up Q&A bank alongside the existing Shopify Q&A doc — not yet executed.
- The "give me an example of when you'd say no to Hydrogen" follow-up needs a queued story (Kindwater headless-middleware or Functions-vs-Scripts tradeoff) — flagged but not written out as a script.
- Performance answers reference the Artifi configurator and the proposed web-worker offload — verify whether that's actually how the project was built (or what you'd do today) before saying it as fact.
- The "what's the worst performance bug you've fixed?" prompt needs one real, defended story — currently a placeholder.

## Linked entities

[[nikita-ilin]] · [[speroteck]] · [[shopify-plus]] · [[hydrogen]] · [[liquid]] · [[oxygen]] · [[checkout-ui-extensions]] · [[customer-account-ui-extensions]] · [[shopify-functions]] · [[bigcommerce-catalyst]] · [[core-web-vitals]] · [[proj-artifi-configurator]]
