# RQ3: model-feedback discovery

This directory contains the RQ3 campaign template. See the [RQ3
walkthrough](../../examples/rq3/README.md) for the paper context and run commands.

RQ3 starts with ten world implementations from collection A or B, their fixed feedback
summaries, the three shared examples, and a card describing the starting worlds and
difficulty objective. It does not use the nine RQ1/RQ2 semantic profiles. The feedback
policy is `difficulty-feedback@0.9.0`; this release uses the `episodic-sequential@0.7.0`
controller variant. Some historical campaigns used a later controller version; see the
release notes.

```bash
uv run --extra runner python -m scripts.release prepare --rq 3 --agent opencode \
  --id rq3_collection_a_demo --collection a
uv run --extra runner python -m scripts.release preview --campaign campaigns/rq3_collection_a_demo.toml \
  --out artifacts/rq3-a-preview
```

Repeat with `--collection b` and a new ID for an independent second collection. Use
`--agent codex` or `--agent claude` for either of the other two builder adapters. Use
`--agent-seconds 21600` for six hours of agent-session time; feedback and verification
have a separate elapsed-time allowance. This section requires an OpenRouter evaluator
key regardless of the builder adapter; see
[authentication](../../docs/AUTHENTICATION.md).

The reference panel is Sol high, Opus high, and Gemini Flash high. Each evaluates five
instances. A world qualifies as difficult for the panel ("panel-hard") when at least two
evaluators answer at most three of five correctly, with all 15 responses available.
Initial feedback contains two detailed samples per starting world; aggregate scores
represent five. Candidates that pass protected checks are rendered and verified offline
before evaluation. Later agents receive summaries of predictions, correctness, public
rationales, available measures of reasoning effort, and guidance for further generation.

**Visibility:** initial JSON includes `oracle_answer`, predictions and correctness.
These are starting-world labels, not protected answers for a newly submitted candidate.
No initial image files are supplied. New code may render images, and later rejected
candidate directories may contain galleries. See [release
differences](../../docs/RELEASE_NOTES.md) for omitted historical render evidence and
stale wording retained in versioned instructions.

After inspection, follow the root README to authenticate, freeze, commit and run.
