# Qulook

AI-native observability, then infrastructure control — built on **TrueForge** for the [Agent Harness Hackathon](https://www.wemakedevs.org/hackathons/trueforge).

**One-line:** Qulook is the Keel idea under a TrueForge harness: an agent that watches production, explains what broke, and waits for a human before it changes anything.

**Heritage:** Product vision and loop come from [Keel](../keel) (Sazabi-shaped observe → explain → approve → act). The hackathon constraint is non-negotiable: **TrueForge must be the runtime**. Keel’s home-grown agent loop is not the submission.

---

## Mission (hackathon)

> Give the model a license to act — but only after a person says yes.

Judges must see TrueForge doing real work, not sitting under a thin wrapper:

1. **Reach real systems** — tools via MCP (logs, metrics, status), not mocks.
2. **Run code safely** — agent-written investigation code in the TrueForge sandbox.
3. **Stay in control** — pause for human approval before restart / rollback / any irreversible step.

Domain: **incident responder**. One narrow job done end to end beats a half-finished platform.

| | |
|---|---|
| **Event** | The Agent Harness Hackathon (WeMakeDevs × TrueFoundry) |
| **Window** | August 24–30, 2026 · Mon 8 AM → Sun 8 PM London |
| **Team** | Solo or up to 4 |
| **Submit by** | August 30, 8:00 PM London — public repo, ~3 min demo, short write-up |
| **Tracks we aim for** | Double-O (TrueForge), Savile Row (UI), optionally Q Branch (Qodo) |

---

## The loop we are building

```
logs / signals land
         → detector or seeded anomaly (no monitor config)
         → TrueForge agent investigates (MCP tools + sandbox)
         → Slack / UI card: impact · root cause · recommended action
         → humans debug in the thread (same agent, same harness)
         → agent proposes an infra action
         → TrueForge holds for approval  ← hero moment in the demo
         → human approves → action runs → outcome + audit
```

People never need a dashboard. Chat / UI and MCP are doors into the **same** harness session.

**Philosophy (from Keel / Sazabi, we keep this):** chat is the UI; logs are the primitive; static monitors are a failure mode. If a feature needs a threshold config screen, it is off-brief for this week.

---

## What “done” means for the demo (~3 minutes)

| Beat | What the judge sees |
|---|---|
| 1. Something breaks | Seeded error spike (or real ingest) |
| 2. Agent acts | TrueForge session: MCP tool calls against logs/metrics |
| 3. Sandbox | Agent runs investigation code in the harness sandbox (visible) |
| 4. Card | Impact, root cause, recommended action |
| 5. Gate | **Licence required** — pause before rollback / restart |
| 6. Approve | Human clicks; action executes; error rate / status recovers |

If it would work just as well as a chat box with no tools, sandbox, or approval, it does not qualify.

---

## Two acts (same as Keel — hackathon ships Act I + a thin Act II)

### Act I — Observe and explain (Sazabi-shaped)

| Pillar | Hackathon “done” |
|---|---|
| **Autonomous alert** | One detector family (error spike) → investigated card |
| **Conversational debugging** | Follow-ups in the same TrueForge session / Slack thread |
| **Harness-visible work** | MCP + sandbox + session continuity across the demo |

Skip for this week: full six detectors, ClickHouse, web dashboard, frustrated-user / cost signals.

### Act II — Infrastructure control (gated)

Qulook’s brain decides *what* should happen. TrueForge enforces the pause. Execution stays Tier 0 only.

```
TrueForge agent → approval gate → operator / stub action → flags / restart / rollback
```

| Tier | Hackathon stance |
|---|---|
| **0 — suggest only** | Default. Demo lives here. |
| **1 — playbooks** | Out of scope unless Stage 1 is rock solid early |
| **2 — bounded autonomy** | Not this week |

Risk order if we have spare time after the demo loop: feature flag toggle → pod restart → deploy rollback. Never skip the gate.

---

## TrueForge is the product surface

We do **not** reimplement an agent harness. TrueForge handles:

- MCP tool connections (including OAuth where needed)
- Sandboxed execution of agent-written code
- Human approval before sensitive actions
- Subagents (optional stretch: parallel log + deploy lookup)
- Session survival across reconnects
- Model choice (bring your own key online; OpenAI credits if in SF)

**Qulook owns:** domain logic — what to detect, which tools to expose, how the card reads, which actions exist, how Slack/UI renders the pause.

Drive TrueForge via chat UI, HTTP API, or TypeScript library — pick one primary path for the demo and stick to it.

```bash
# Day-one harness (from hackathon briefing)
npx @truefoundry/trueforge
```

---

## Week path (build for the demo, not the roadmap)

Each stage has a **feelable demo**. Stop when the 3-minute video is honest.

| Stage | Focus | Demo when done | Priority |
|---|---|---|---|
| **0. Harness up** | TrueForge running; model + first MCP tool | Agent reaches a real tool in the UI | Day 1 |
| **1. Live incident loop** | Seeded spike → investigate → card → approval → stub action | Full loop on camera | Days 1–3 |
| **2. Control UI** | Slack or simple UI: doing / waiting / done; gate before irreversible | Savile Row–ready screen recording | Days 2–4 |
| **3. Real tool depth** | Logs + one more signal (deploy/version or service stats) | Root cause names a change or service with evidence | Days 3–5 |
| **4. Sandbox proof** | Agent bisects / queries in sandbox, not only chat | Judge sees sandbox step | Days 3–5 |
| **5. Qodo trail** *(optional track)* | Qodo from first commit; PR-based merges | Review history exists | All week |
| **6. Stretch** | Subagent research, second action type, undo | Only if 1–4 are filmed | Days 6–7 |

**Cut list (Keel Stage 3–13):** silent-failure detector suite, incident memory v1, full CLI parity, GitHub PR handoff, customer-side operator, K8s/CI/VM tiers, SSO, dashboards.

Detailed checklist: [`qulook-notes.md`](./qulook-notes.md).

---

## Judging map (six criteria, equal weight)

| Criterion | How Qulook scores it |
|---|---|
| **Potential impact** | On-call would hand over “investigate this spike and roll back if needed” |
| **Creativity** | Observability agent with a hard license-to-act — not another FAQ bot |
| **Technical excellence** | Narrow loop complete, reliable, readable public repo |
| **Use of sponsor tools** | TrueForge central (tools + sandbox + approval); Qodo if chasing that track |
| **Control and safety** | Sandbox + explicit human gate in the demo video |
| **Presentation** | 3-minute story: problem → agent working → where the harness fits |

One team, one prize track win max — enter everything; pick narrative for Double-O + UI.

---

## Submission checklist

- [ ] Public GitHub repo (this one: `Arnab-Afk/qulook`) with a stranger-runnable README
- [ ] Agent runs on **TrueForge**; harness visibly does the work
- [ ] Demo video ~3 minutes (gate moment mandatory)
- [ ] Short write-up: job given to the agent, how TrueForge is wired, what broke
- [ ] No secrets / personal data in repo or video
- [ ] Build during hackathon week (Keel is inspiration + optional scaffolding, not a paste of an old product)
- [ ] Qodo installed day one + PR trail *(only if Q Branch)*
- [ ] Blog / social optional (Field report / Radio traffic)

---

## Architecture (hackathon target)

```
                    ┌─────────────────────────────────────┐
   logs / seeds ──► │  Qulook domain (detect, card, acts) │
                    └──────────────┬──────────────────────┘
                                   │ MCP tools · actions · prompts
                    ┌──────────────▼──────────────────────┐
                    │           TrueForge harness           │
                    │  model · MCP · sandbox · approval     │
                    │  subagents · sessions                 │
                    └──────────────┬──────────────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                    ▼
         Slack / UI            MCP clients         Stub / real
         (approval UX)         (logs, metrics)     operator actions
```

**Reuse from Keel when it saves time:** event/alert shapes, detector ideas, Slack card copy, approval UX patterns, seed scripts.  
**Replace:** Anthropic-only agent loop, anything that hides TrueForge.

Shared contracts should stay small: `Event`, `Anomaly`, `Alert`, `Action`. Transports (Slack, demo UI) are adapters.

---

## Best practices (from the briefing — we treat these as law)

1. **The harness has to be doing real work** — qualify or DQ.
2. **One job an agent can finish** — error spike → approve rollback/restart.
3. **Open PRs from the first commit** — if Qodo track.
4. **Put the approval gate in the demo** — film the pause.
5. **Ship a repo a judge can run** — README that works on someone else’s machine.

---

## Repo layout (intended)

```
qulook/
  README.md           ← you are here
  qulook-notes.md     ← day-by-day build / validate
  # domain + adapters land during the week
  # TrueForge is runtime (npx / compose), not reimplemented here
```

---

## Run (fill in as Stage 0 lands)

```bash
# Harness
npx @truefoundry/trueforge

# Domain (once scaffolding exists)
cp .env.example .env   # model key; Slack if used
# npm install && npm run …  (TBD Stage 0)
```

Online: bring your own model API key. TrueForge talks to any OpenAI-compatible provider.

---

## Name

**Qulook** — look into prod with a license to act. Not affiliated with James Bond branding used by the hackathon theme; we reuse only the “license to act / approval” metaphor in product copy where it helps the demo story.

---

## Appendix — Keel → Qulook mapping

| Keel | Qulook (this week) |
|---|---|
| Own agent loop (Anthropic) | TrueForge harness |
| Act I full Sazabi parity | One detector + investigate + card |
| Act II tiers 0–2 + operator | Tier 0 only + visible TrueForge approval |
| Slack / CLI / MCP as equal doors | One primary UI + MCP tools on the harness |
| Multi-week roadmap | Stages 0–4; stretch 5–6 |

**Bottom line:** same product soul as Keel; different runtime; hackathon-sized slice; approval is the plot twist.
