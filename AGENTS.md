# Working with this repository

DualLoop runs the three discovery experiments described in the root README. Start with
[README.md](README.md), then the relevant guide in [examples/](examples/README.md).
These instructions are for an assistant operating or maintaining this repository. The
instructions delivered to a world-generating agent are separate, versioned files under
`foundry/`.

## Prepare and inspect a run

Run commands from the repository root. Install the environment with `uv sync --locked
--extra runner --group dev`. Read [setup](docs/SETUP.md) before building Docker images
or checking the runtime.

For a first RQ1 configuration:

```bash
bash examples/rq1/prepare.sh --agent codex --profile tracing --id rq1_review
uv run --extra runner python -m scripts.release preview \
  --campaign campaigns/rq1_review.toml --out artifacts/rq1_review_preview
```

Preparation and preview do not call a model. Read the generated `instruction.md` and
`environment/starter/` files before proposing a run. Choose a new campaign ID and output
path for each attempt. Never overwrite existing run evidence.

A paid authentication check or campaign requires an explicit operator request. Follow
[authentication](docs/AUTHENTICATION.md); do not print credentials, put them in Git, or
include them in a report. Review, freeze, and commit the configuration before launching.
All other source changes and newly prepared configurations must also be committed or
moved out of the working tree. The runner requires a clean working tree.

After a run, report the campaign ID, configuration, output path, completed checks, and
failures. Distinguish a prepared configuration from a completed campaign. The runner
does not resume interrupted campaigns. RQ4 training and the paper's historical 200-scene
replay analysis are outside this release.

## Maintain the release

Keep generated outputs under ignored `artifacts/`. Do not add manuscript sources,
historical agent traces, credentials, personal paths, or account identifiers. The
approved Figure 1 image is the sole documentation-image exception.

Preserve the answer logic, protected checks, and frozen profile and strategy text.
Changing these changes the experiment; do not edit them to make a failing run pass. Use
plain language in reader documentation and retain exact technical identifiers where
needed to locate code or reproduce a command.

Run `uv run --extra runner pytest -q` before committing. For runtime changes, also run
the model-free Docker smoke test. Before packaging or publishing the release, run `uv
run --extra runner python -m scripts.audit_release`. The scan rejects tracked local
campaign configurations; it checks the distribution, not the working repository after
new campaigns have been registered.

Preserve anonymous commit metadata and do not import another repository's history.
Finish with `git status --porcelain` and account for any remaining changes.
