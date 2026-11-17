# Swarm Frame

The orchestration layer for AI red-team harnesses — because the model was never the bottleneck. The coordination was.

## The insight

Attackers coordinate. Defenders don't — or rather, their tools don't. Every AI security tool on the market runs solo: one agent, one harness, one run, one narrow slice of the engagement. Meanwhile the other side specializes, hands off, and shares tooling as a matter of course.

I tested this directly. Three different open-source AI red-team harnesses, same model, same target class — wildly different results. The model wasn't the variable. The orchestration was. Nobody was building the layer that lets these harnesses work as a team, so I'm building it.

## What it is

Swarm Frame is role-based agent teams for offensive security testing:

- **An operator interface** that takes plain language and turns it into a plan.
- **A reasoning brain** that understands the target, the kill chain, and the MITRE ATT&CK phases in play.
- **A living knowledge library** that grows with every session — every finding makes the next engagement sharper.

The core mechanism is **harness-to-harness handoff**: a web-application testing harness can call a network-exploitation harness's operators mid-engagement, the way a real red team hands off between specialists. No single harness has to do everything badly. Each does what it's built for, and the frame coordinates.

It runs on **local open models** — fully air-gapped if needed. No client or target data ever touches a cloud API. That's not a preference, it's a hard requirement for real engagements, and it's the reason cloud-based tools can't do this job.

## What's proven vs. what's building

Proven:

- The lab is built and networked — two-box local rig, model servers live.
- Real agent-harness execution, including a full AI-driven kill chain that produced 32 findings (T3MP3ST).

Building now:

- The A2A wire and vision harness in `docs/a2a-vision-harness.md` — the seats (Big Head, Glimmer) calling each other as tools, with eyes on the target. That's the build in progress, not a finished claim. The doc says what's verified and what's still to confirm.

## Defensive use only

This is defensive infrastructure, built in the open. Every technique it automates is one defenders can now test against systematically — instead of hoping the annual pentest caught it. Authorized testing only.

## What never goes in this repo

No bounty-target trajectories, findings, target URLs, credentials, proof-of-concepts, or logs. That material is quarantined and only enters any training corpus after policy review, redaction, and written consent. If you find any of it here, that's a mistake — report it and it gets pulled.

## The ask

Built toward an AI Grant application ($20,000: $10k GPU hardware for the lab, $3k cloud GPU burst for evaluation runs, $7k three months of full-time build runway). If funded, a blog post is owed within 90 days.

## License

MIT — see `LICENSE` when added.
