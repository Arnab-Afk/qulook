# Qulook — Hackathon build notes

Working checklist for the TrueForge week. High-level product brief: [`README.md`](./README.md).

**Meta-rules**

1. TrueForge is the agent runtime. Do not ship a parallel agent loop as the demo path.
2. One `Alert` / `Action` shape. Slack or UI only renders; detectors only emit anomalies.
3. Every irreversible step goes through TrueForge human approval (Tier 0).
4. Film the gate. If it is not in the video, judges will not count control/safety.

---

## Day map (Aug 24–30, 2026)

| Day | Goal | Exit criteria |
|---|---|---|
| **Mon 24** | Stage 0 + start Stage 1 | TrueForge up; one MCP tool called; repo public; Qodo on *(if chasing Q Branch)* |
| **Tue 25** | Stage 1 spine | Seeded spike → investigate → structured alert fields |
| **Wed 26** | Stage 1 + 2 | Approval pause + stub execute; UI/Slack shows waiting |
| **Thu 27** | Stage 3–4 | Second signal or sandbox step visible; tighten demo script |
| **Fri 28** | Harden + record | Reliable 3-minute take; README runnable |
| **Sat 29** | Polish / SF optional | Blog draft; social clip; fix judge-path bugs |
| **Sun 30** | Submit before 8 PM London | Repo + video + write-up form |

---

## Stage 0 — Harness up

**Build**

- Run `npx @truefoundry/trueforge` (or compose stack if needed for “real work”).
- Connect a model (own key).
- Connect at least one MCP server Qulook will use (logs query, or a fixture MCP that returns seed incidents).
- Public repo README: how to start harness + domain.

**Validate**

- Fresh machine / clean shell: harness starts.
- In TrueForge UI (or API): tool call succeeds and returns data.
- No secrets committed.

**Foundation**

- All later tools register as MCP (or TrueForge built-ins), not ad-hoc SDK calls that bypass the harness.

---

## Stage 1 — Live incident loop  ← primary ship target

**Build**

1. Seed or ingest an error spike for one service (fixture script is fine).
2. TrueForge agent prompt / skill: investigate impact, root cause, recommended action.
3. Produce an `Alert`-shaped result (title, severity, impact, root cause, recommended action).
4. Propose one action (e.g. `rollback_deploy` or `restart_service`) with params.
5. TrueForge **holds for approval**; on approve, run a **stub** that flips status / recovers the seed.

**Validate**

- One spike → one investigated outcome (no spam cards on re-poll).
- Without approval, stub never runs.
- With approval, demo “recovers.”
- Uncertain model still fills all four card fields (say so; do not omit).

**Foundation**

- Stub actions are real `Action` objects (`dry_run` → approve → `execute` → audit). Swapping stub for a real API later must not change the gate.

---

## Stage 2 — Control UI (Savile Row)

**Build**

- Surface that shows: what the agent is doing, what it is waiting on, what it did.
- Approval **before** irreversible step, not an after-the-fact toast.
- Prefer Slack thread + buttons if Keel Slack patterns transfer fast; else a minimal web/chat shell driven by TrueForge.

**Validate**

- Stranger can follow the demo without narration crutches.
- Screen recording alone makes the gate obvious.

**Foundation**

- UI is an adapter. No second copy of tool logic.

---

## Stage 3 — Tool depth

**Build**

- Logs MCP (or ingestion → query tool) plus one correlating signal: deploy/version list **or** per-service error stats.
- Agent must cite evidence in root cause (version, timestamp, service name).

**Validate**

- Seed deploy then spike → root cause mentions the change when present.
- No deploy in window → agent says so (no hallucination).

---

## Stage 4 — Sandbox proof

**Build**

- At least one investigation step runs as agent-written code in TrueForge sandbox (e.g. aggregate rates, bisect window, parse fixture).
- Demo script calls out “running in sandbox.”

**Validate**

- Judge can see sandbox in UI/logs of the harness session.
- Wrong code cannot touch host / prod credentials.

---

## Stage 5 — Qodo (optional track)

**Build**

- Install Qodo on day one (free for OSS).
- All work via pull requests; address or reply to findings before merge.

**Validate**

- PR history spans the week, not a single last-hour PR.
- Judges can read the trail.

**Note:** Skip entirely if it slows Stage 1. Double-O does not require Qodo.

---

## Stage 6 — Stretch only

- Subagent: parallel “logs” + “deploys” gather, merge into one root cause.
- Second action type (flag toggle) still Tier 0.
- Undo after execute.
- Blog post + social clip for open prizes.

Do not start Stage 6 until Stages 1–2 are recorded.

---

## Demo script (draft — tighten Friday)

1. **Setup (10s):** “On-call for checkout. No monitors configured.”
2. **Spike (20s):** Run seed; show errors rising.
3. **Investigate (60s):** TrueForge session — MCP tools fire; sandbox step; card fills.
4. **Gate (40s):** Recommended rollback; harness asks for licence / approval; **do not skip this**.
5. **Resolve (30s):** Approve; stub runs; status recovers; point at audit / session log.
6. **Close (20s):** “Qulook = Keel’s idea; TrueForge = license to act.”

Total ~3:00. One take with backup.

---

## Submission packet

| Artifact | Owner | Due |
|---|---|---|
| Public repo + README | — | Before form |
| Demo video (~3 min) | — | Before form |
| Write-up (job, TrueForge wiring, what broke) | — | Form |
| Optional blog link | — | Form |
| Optional social tags: WeMakeDevs, TrueFoundry, Qodo | — | During week |

Form deadline: **August 30, 2026, 8:00 PM London**.

---

## Intentionally not this week

- Full Keel detector suite and memory system
- Customer-side credential operator
- K8s / CI / VM action packs
- Web observability dashboard
- Auto-remediation (Tier 1+) without a human in the demo
- Rebranding distraction — name is Qulook; story is incident + approval on TrueForge

---

## Open decisions (resolve Day 1)

| Decision | Options | Pick when |
|---|---|---|
| Primary UI | TrueForge chat only · Slack · tiny custom UI | After Stage 0 |
| Action stub | In-process fake · docker sidecar · script flip | Stage 1 |
| Log source | Fixture MCP · Keel ingestion reuse · Loki | Stage 0–1 |
| Qodo track | Yes / no | Monday morning |

Update this file as decisions land. Do not fork a second plan doc.
