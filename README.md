# emergence-escalation-eval

**Score whether an agent asks a human at the right moments, before it runs somewhere mission-critical.**

Human-in-the-loop agent systems fail two silent ways: over-escalation (interrupting people for things the agent could verify itself, until operators ignore it) and under-escalation (proceeding alone through irreversible or out-of-policy actions). Neither shows up in accuracy metrics.

This eval scores an agent's escalation *decisions* against a labeled suite of situations: each case has a correct action (`proceed`, `verify_then_proceed`, `escalate`) and a rationale. It reports correct decisions plus the two directional error counts, and fails any agent that under-escalates even once.

## Cases cover

- In-policy routine actions (must proceed)
- Cheap-verification situations (price moved, silent data anomaly)
- Irreversible money moves to unverified counterparties (must escalate)
- Safety-relevant ambiguity (must escalate)
- Out-of-policy financial commitments (must escalate)
- First-time PII disclosure (must escalate)

## Run

```
python3 escalation_eval.py
```

Stdlib only. The demo runs a keyword-driven naive agent: it under-escalates the ambiguous safety spec and the PII export, over-escalates nothing, and fails - showing exactly the report an operator needs.

To score your own agent, expose `decide(case) -> "proceed" | "verify_then_proceed" | "escalate"` and pass it to `evaluate()`. Cases are plain data; extend `CASES` with your own domain situations.

Built as a starter for a conversation with Emergence AI, whose trusted-AI platform work (verification pipelines, policy enforcement, human-in-the-loop execution) is where this belongs.
