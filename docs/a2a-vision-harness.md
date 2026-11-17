# A2A wire + vision harness

How the two lab seats talk to each other, and how they see.

## The seats

- **Big Head** — Minisforum box, Qwen3-Next-80B. The heavy lifter / bounty-run driver.
- **Glimmer** — GEEKOM box, Muse-Glimmer-30B. The chat seat.

Both run under Hermes. Both get brain access (shared knowledge library).

## Agent-to-agent (A2A)

- Hermes has an a2a plugin (v1.0), off by default. Agents call each other as tools.
- The important split: **agents speak A2A, models don't.** The peer is the Hermes agent driving the model, not the model itself.
- Wire-up: turn a2a on in Hermes on both boxes, list each side as a peer in the other's `config.yaml`.
- OpenCode has **no native A2A** — it needs a wrapper (`opencode-a2a` on PyPI, or `a2a-opencode` on npm). Hermes-to-Hermes needs none.
- Security: an A2A endpoint is a front door by design. It stays on the lab net behind the Pi, never pointed at the open internet without auth.

## Vision — built in, verify first

Belief to confirm, not a fact yet: the newer uncensored Glimmer build and the Big Head model both have vision built in, so no separate vision model is needed — just wire the A2A and the eyes are already there.

How to know for sure: check whether each model has its vision file, the **mmproj** — that's the piece that turns vision on in llama-server. If they have it, you're set. If not, that's what a separate vision model was for.

- CyberTiel (uncensored 35B coder) is text-only and down right now. If it had vision it would be the pentest-harness pick on the Minisforum — it doesn't, so Big Head holds that seat.

## What this unlocks

Once the wire is up, Glimmer can hand a screenshot / screen state to Big Head (or the other way), and the harness agent can act on what it sees instead of working blind. That's the "eyes + hands" step the whole lab has been building toward.

## Status

Plan set 2026-11-14/15, GitHub connected 2026-11-17. Wiring is at the operator's move — not done yet, and this doc gets updated with what actually worked when it lands.
