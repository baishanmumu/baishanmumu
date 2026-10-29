# AgentLens

> A reproducible evaluation and debugging workbench for tool-using LLM agents.

**Status: Concept stage. No implementation or benchmark results are claimed yet.**

## The problem

An agent demo can look convincing while hiding brittle behavior. A small change to a prompt or model may alter tool selection, arguments, stopping behavior, or the final answer. Without a fixed task set and step-level traces, it is hard to tell what improved and what regressed.

AgentLens is a proposed project for making those changes inspectable and repeatable.

## Proposed workflow

```text
Task set → Agent runner → Run trace → Replay & evaluation → Version comparison
```

- **Define tasks** with an input, available tools, and explicit success checks.
- **Record runs** with model and prompt versions, tool calls, observations, timing, and token usage.
- **Replay deterministically** with recorded or mocked tool responses, so debugging does not depend on live services.
- **Compare versions** using task success, tool selection and argument validity, answer quality checks, latency, and token cost.
- **Inspect failures** by step, with the trace and evaluator evidence beside each result.

## MVP boundary

Start with a small local benchmark for a knowledge assistant that can search a fixed document set and use a calculator. Use synthetic documents and fixture-backed tools so runs are safe, repeatable, and do not require external credentials.

The first version should answer four questions:

1. Did the agent complete the task according to explicit checks?
2. Did it choose an allowed tool and produce valid arguments?
3. Where did a failed run go off track?
4. Did a prompt or model change improve the baseline, and what did it cost?

## Evaluation design

Prefer deterministic checks where possible:

| Signal | Example check |
| :--- | :--- |
| Task completion | Expected facts or structured fields are present |
| Tool choice | The selected tool belongs to the task's allowed set |
| Tool arguments | Arguments satisfy a JSON Schema and task constraints |
| Grounding | Required claims cite the fixture document that supports them |
| Efficiency | Tool-step count, elapsed time, and token usage are recorded |

Use model-based judging only as an optional, separately reported signal; do not mix it with deterministic pass/fail results.

## Initial architecture

```text
Task definitions
      │
      ▼
Agent runner ───── Model adapter
      │
      ├──────────── Fixture-backed tools
      ▼
Trace recorder
      │
      ├──────────── Replay engine
      ▼
Evaluator ──────── Comparison report
```

Keep the first implementation local-first: versioned task files, a small run store, and a readable report. Select frameworks and storage after the first end-to-end slice is working.

## Milestones

- [ ] Define a compact task format and a first set of synthetic cases.
- [ ] Implement one agent run with fixture-backed search and calculator tools.
- [ ] Record structured traces and add deterministic replay.
- [ ] Add task, tool-call, and efficiency metrics.
- [ ] Compare a baseline with one controlled prompt or model change.
- [ ] Publish the implementation, reproducible commands, and measured results.

## What belongs here after implementation

The finished project should include an architecture diagram based on the actual code, a runnable demo, a small public evaluation set, baseline results with the test conditions, known limitations, and a clear record of personal contributions.
