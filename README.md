# ◊ FallSeed · HR Firm

**A Progressive Web App (PWA) for UK HR firms. Single HTML file. LLM-agnostic build engine. Forkable to any vertical.**

Live: <https://sjgant80-hub.github.io/fallseed-hr/>

---

## What it is

One HTML file (~80 KB) that runs in any modern browser. Installs to desktop and phone as a Progressive Web App. Works offline. Ships with:

- **Four working HR tools already live on GitHub Pages** — anchor (employee register · holiday · absence · reviews), onboard (7-step new starter wizard), paper (8 document templates including S.1 statement + settlement agreement), practice (payroll PAYE/NI/pension + HMRC returns).
- **A build engine** — describe a new HR tool in plain English, the cascade calls any LLM you've configured (WebLLM in-browser, ollama/LM Studio local, free cloud tiers, BYOK paid), streams the HTML back, you download a working tool.
- **A Fork Seed mechanic** — edit the seed in-place (rename vertical, swap T0 rules, swap base-tool URLs, rewrite build prompt), click Download, get a complete `fallseed-{vertical}-v1.html` you can ship as a seed for any other industry.

---

## Install as a PWA

| Platform | How |
|---|---|
| **Chrome desktop** | Click the install icon (⬇) in the address bar, or menu → "Install FallSeed" |
| **Edge desktop** | Same — install icon in the address bar |
| **Chrome Android** | Menu → "Install app" |
| **Safari iOS** | Share → "Add to Home Screen" |

You get a desktop icon, its own window, full offline support after first load. No app store. No tracking. The whole product is `index.html`.

---

## The four base tools

These are already shipped and live; FallSeed configures and links to them.

| Role | Tool | URL |
|---|---|---|
| anchor | fallhr | <https://sjgant80-hub.github.io/fallhr/> |
| onboard | fallhronboard | <https://sjgant80-hub.github.io/fallhronboard/> |
| paper | fallhrpaper | <https://sjgant80-hub.github.io/fallhrpaper/> |
| practice | fallhrpractice | <https://sjgant80-hub.github.io/fallhrpractice/> |

They mesh-sync through the browser's BroadcastChannel (`fall-hr`) when two or more are open in the same browser. Onboard an employee in fallhronboard → they appear automatically in the other three.

---

## LLM cascade

Eight providers across four tiers. The cascade tries them in priority order, skips ones unavailable or rate-limited, uses the first that responds.

| Tier | Provider | Cost | Note |
|---|---|---|---|
| T1 (sovereign) | WebLLM | free | In-browser via WebGPU · ~2GB first download · zero network after |
| T2 (sovereign) | ollama | free | localhost:11434 · auto-detected on boot · `OLLAMA_ORIGINS=* ollama serve` |
| T2 (sovereign) | LM Studio | free | localhost:1234 · auto-detected · enable CORS in Server tab |
| T3 (free cloud) | Groq | free | 30 req/min · Llama 3.3 70B · fastest cloud inference |
| T3 (free cloud) | OpenRouter free | free | 200 req/day · multiple free models |
| T3 (free cloud) | Google AI Studio | free | Gemini 2.5 Flash · 1M tokens/day free |
| T3 (free cloud) | Cerebras | free | Llama 3.3 70B at ~2200 tok/sec |
| T3 (paid) | Anthropic | ~£0.03/tool | Claude Sonnet 4 · best codegen quality |

**Your key, your bill, your bandwidth.** Each LLM call goes browser → that provider's API directly. No intermediary server. Keys live in your browser's IndexedDB only.

---

## Fork Seed (the strategic mechanic)

The Packager tab lets you edit every part of the seed:

- Vertical name and mesh channel
- 4 base tool URLs and purposes
- T0 rules (regex → answer pairs encoding the vertical's regulatory knowledge)
- Compliance checklist items
- Locked document clauses
- The LLM build prompt that generates new tools

Click **Download fallseed-{vertical}-v1.html** and you have a complete new PWA seed for any vertical — clinic, vet, recruit, construction, accountancy, marketing, pharmacy, dentist, education, hospitality, transport. Five minutes per vertical.

---

## Architecture

| Layer | Tech | Sovereign? |
|---|---|---|
| Storage | IndexedDB (per tool) | yes — data never leaves device |
| Mesh | BroadcastChannel (`fall-hr`) | yes — in-browser only |
| Audit | P3 chain (prevHash + SHA-256) | yes — verifiable locally |
| LLM | T1 → T2 → T3-free → T3-paid cascade | T1/T2 fully sovereign; T3 your choice |
| UI | Single HTML, no framework, no build step | yes — fork or read the source |
| PWA | Inline manifest (data: URL) | yes — no separate manifest file |
| Licence | MIT | yes |

---

## Use cases

1. **HR firm owner** — install, provision firm config, use the four base tools daily, build the fifth or tenth tool you need on demand
2. **Software developer** — fork the seed for any vertical you have domain knowledge in, ship as your own MIT product
3. **Anyone learning AGI-adjacent architecture** — the source is one HTML file with no minification, clearly commented, demonstrates LLM cascade, BroadcastChannel mesh, PWA install, IndexedDB persistence, audit chain, and seed-generative-substrate patterns

---

## Licence

MIT · Simon Gant · prime 1049 · ◊·κ=1.


## What kind of seed is this?

A **level-0** seed in the FallSeed family. Built on the Fork Seed primitive — see [the spec](https://www.ai-nativesolutions.com/spec.html) for the four invariants of replication, the SEED schema, and the six-step fork protocol that every conforming seed implements.
