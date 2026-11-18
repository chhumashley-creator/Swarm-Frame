# Swarm Frame — Architecture

Status as of 2026-11-18. Living doc — updated as the stack changes.

## The idea

The model was never the bottleneck. Coordination was. Swarm Frame wires multiple agents together so a pentest run survives the limits of any single model: a cheap cloud brain does the heavy thinking, local models finish the job, and the run itself lives on disk — not in any one context window.

## The seats

- **Cloud brain — DeepSeek V4.1 Flash Uncensored** (Experiential Labs, OpenAI-compatible API). The heavy hitter. 1.05M context, 393K max output. Drives the harness.
- **Big Head — local 80B** (Minisforum, 131K context). The finisher. Picks up a run from its checkpointed report when the cloud brain taps out.
- **Glimmer — local 30B** (GEEKOM, 65K context). The front seat. Coordinates, calls the other agents.

## The wire — A2A

Agents talk agent-to-agent over A2A (Hermes gateway, port 9900, bearer auth). Each box runs its own gateway as a user service, configured by environment (systemd drop-in), not by the setup UI.

- Glimmer presents as `glimmer`, Big Head as `hermes-omarchy`.
- Peers are named and token-authenticated on both sides; a trusted-peers allow-list is required for a non-loopback bind.
- The lab has two networks; A2A traffic rides the direct cable (10.60) — the general lab net (10.50) firewall drops port 9900. Glimmer reaches Big Head at `10.60.0.1:9900`, Big Head reaches Glimmer at `10.60.0.2:9900`.
- Proven 2026-11-18: Glimmer ran `a2a_discover` against Big Head and got the full agent card (JSONRPC 1.0, streaming + push, ~40 toolsets incl. a2a, browser, computer_use, file, terminal, vision, web). First real `a2a_call` task: pending.

## The fallback — checkpointing

Tokens burned is not context held. A run can burn millions of tokens while never holding more than its window at any moment. So the harness checkpoints findings to the shared brain/disk as it goes. When the cloud brain exhausts credits or context, Big Head reads the partial report and finishes from disk. No checkpoint, nothing to hand off.

- Context is working memory. The run lives on disk.
- Proven pattern: a full kill chain already finished on this stack (32 findings) by working step by step and writing it out.

## Token budget — DeepSeek Uncensored (researched 2026-11-18)

- Pricing (promo): $0.23 input / $0.90 output per 1M tokens. 1 credit = $0.01. Central blend ≈ $0.36 per 1M tokens.
- NOT free: the free tier's 1M tokens/day covers only the stock models. The uncensored model bills credits on every call.
- How far is 1M tokens: about half a light web-app job, ~18% of a quick Strix pass, ~5% of a full HTB budget. Barely a start on a long run.
- A 20M-token run ≈ $7.20. $20 ≈ 56M tokens ≈ 26 runs.
- Limits that cut a run: Pro 10M tokens/day cap, 429 `insufficient_quota` hard stop on credit exhaustion (retrying doesn't clear it), 1.05M context ceiling, ~204 tok/s decode.
- Onboarding credits: the 2,100 figure on the signup screen could not be verified anywhere public — don't budget on it until it's in the account.
- Catch: prompt capture is ON by default. Turning it off / zero-data-retention needs the Pro plan. Captured content may be used to improve hosted services.

## Harness

Undecided. T3MP3ST is proven on this stack (full kill chain, 32 findings). Newer harnesses are under evaluation — the pick gets recorded here when it's made. Whatever the harness, it must checkpoint to disk (see above) and take the cloud brain as its driver.

## Target

Vercel on HackerOne — Next.js, Tier 1 open source. Criticals $5,250–$10,000. Authorized targets only, always.

## Rules

- Authorized testing only: bug bounties and systems we have permission to test. No exceptions.
- No secrets in this repo. No API keys, no tokens, no passphrases, no credentials of any kind.
- The finisher is any second brain — local or cheap API — not one specific model. The architecture is provider-swappable; the durable part is the handoff, not the model.
