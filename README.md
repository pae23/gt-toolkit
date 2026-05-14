# gc-toolkit

A collection of formulas and resources for [Gas City] — the multi-agent workspace manager built on [Beads](https://github.com/steveyegge/beads) issue tracking.

## Design-to-Delivery Pipeline

The headline feature: a formula-driven pipeline that takes a feature from initial idea through to landed code, with multi-model review at every stage.

```
┌─────────┐      ┌─────────┐      ┌─────────┐      ┌───────────┐
│  Spec   │ ───▶ │  Plan   │ ───▶ │  Beads  │ ───▶ │ Delivery  │
│ (1-4)   │      │ (5-6)   │      │ (7-8)   │      │   (9)     │
└─────────┘      └─────────┘      └─────────┘      └───────────┘
  idea →           spec →           plan →           beads →
  reviewed spec    reviewed plan    verified beads   landed code
```

**Spec phase** — Multi-LLM scope analysis (Opus, GPT, Gemini in a 3x3 matrix), interactive brainstorming that turns questions into a validated spec, completeness interview, and 3-model parallel review.

**Plan phase** — Deep codebase analysis via parallel agents, phased implementation plan with file-level mapping, and bidirectional plan-to-spec review.

**Beads phase** — Converts the plan into a fully-structured beads issue hierarchy (epics, sub-epics, tasks with acceptance criteria, validated dependency graph), then verifies coverage with 3-agent bidirectional review.

**Delivery** — sling the beads

Three workflow formulas orchestrate the pipeline: `spec-workflow`, `plan-workflow`, and `beads-workflow`. Each stage can also be run individually. Works with Opus alone, or with all three LLMs for maximum review diversity.

See [formulas/README.md](formulas/README.md) for full stage-by-stage documentation.

## What's here

| Directory | Contents |
|-----------|----------|
| `formulas/` | `.formula.toml` files — the design-to-delivery pipeline. 8 expansion formulas and 3 workflow orchestrators. |
| `scripts/` | Maintenance scripts. **Read the warnings in [scripts/beads-cleanup/README.md](scripts/beads-cleanup/README.md) before use — these scripts permanently delete data from Dolt databases.** |
| `docs/` | Workflow guides and reference material |
| `configs/` | Configuration templates |


## Documentation

- [formulas/README.md](formulas/README.md) — Full pipeline documentation with stage descriptions, diagrams, and usage examples



## Acknowledgements

The brainstorming and plan-writing stages draw inspiration from [obra/superpowers](https://github.com/obra/superpowers/tree/main).
