# RQ2: image-support-steered discovery

This directory contains the RQ2 campaign template. See the [RQ2
walkthrough](../../examples/rq2/README.md) for the paper context and run commands.

RQ2 uses the same nine profile cards as RQ1 and adds image-support guidance. Before each
episode, the controller requests a support category and provides construction guidance.
After submission, it measures the generated world's support and updates its category
record. The internal policy is named `quality-diversity@0.8.0`. RQ2 does not use the RQ3
evaluator panel.

```bash
uv run --extra runner python -m scripts.release prepare --rq 2 --agent claude \
  --id rq2_tracing_demo --profile tracing --agent-seconds 1800
uv run --extra runner python -m scripts.release preview --campaign campaigns/rq2_tracing_demo.toml \
  --out artifacts/rq2-tracing-preview
```

Inspect `DIVERSITY_TARGET.md`, `DIVERSITY_TARGET.json`, and
`tools/preview_qd_descriptor.py` in the preview's starter directory. Targets adapt
across episodes; profile question directions stay fixed. The nine categories combine
three scales (focal, regional, distributed) with three shapes (compact, pathlike,
multipart).

Use `--profile all --agent-seconds 21600` to prepare nine separate six-hour campaigns.
Select `--agent codex` or `--agent opencode` to change the supported builder preset.

After inspection, follow the root README to authenticate, freeze, commit and run.
