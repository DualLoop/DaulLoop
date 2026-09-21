# Validation

Local checks used macOS ARM64, Python 3.12.9, Docker 29.7.2, Docker Compose 5.4.0, and
Harbor 0.1.44. Checks included two short authentication tests and two bounded live
discovery campaigns, using Codex and Claude Code. RQ3 was checked locally without
calling evaluator APIs. These are release checks, not paper results.

## Completed checks

| Check | Result |
| --- | --- |
| Unit and integration tests | 356 passed |
| Release ZIP, extracted into a fresh environment | 356 tests passed; environment and model-free Docker checks also passed |
| Experiment and agent configurations | First-episode inputs generated for all three experiments with all three agents |
| RQ1 and RQ2 profiles | All nine cards included, with all five question directions |
| RQ3 starting collections | Both ten-world collections passed content-hash checks and input preparation |
| Starting-world tests and rendering | All 20 original test suites and 32-scene render/inverse checks passed in Docker |
| Protected-test isolation | Normal episode inputs exclude protected tests and the smoke-test solution |
| Docker smoke tests | RQ1, RQ2, and RQ3 (both starting collections) passed all four isolation checks and all five fixed-example verifier checks |
| Live Codex authentication | Fixed output file verified; session reported `gpt-5.6-sol` with `max` reasoning; about 50 seconds including setup and verification |
| Live Claude authentication | Fixed output file verified; response reported `claude-opus-5`; about 35 seconds including setup and verification |
| Live RQ1 campaign | Codex, Tracing profile, 20-minute agent budget: one episode completed and one world retained; all protected and isolation checks passed |
| Live RQ2 campaign | Claude Code, Measurement profile, 30-minute agent budget: two episodes, one retained world matching the requested support category |
| CLI installation | Codex 0.147.0, Claude Code 2.1.224, and OpenCode 1.18.11 installed in Docker; command options checked offline |
| Shared examples | Twelve scenes per world rendered and inverse-verified, producing 72, 60, and 60 question records |
| Example scripts | All 18 RQ1/RQ2 profile configurations and both RQ3 collections prepared, previewed, frozen, and committed in the extracted checkout; working tree clean |
| Documentation | Links in all 77 Markdown files and syntax in 49 documented shell blocks checked; Figure 1 visually inspected |
| Source comparison | All 140 starting-world source/test files and five protected verifier files match the source bytes; shared example logic is unchanged |
| Release scan and maintained-code lint/format checks | Passed; frozen snapshots retain their original style |

Lint and format checks cover `scripts/`, `src/`, `tests/`, `worlds/`, and `harbor/`.
Whole-tree checks also include frozen snapshots and report existing style issues; those
files were not reformatted for this release.

The five-file reference snapshots differ from the source only in three TOML comments
that rename the package. The release scanner checks for personal paths, credential
patterns, excluded files, and tracked files outside the public file list. It permits the
README figure only at its reviewed path and content hash. The scan cannot establish
anonymity against external knowledge or code comparison.

Regression tests cover missing isolation checks, failed verifier scores, accidentally
tracked files, symbolic links, and relative paths in launch commands. Candidate tests
run against temporary copies because some write local check files. The original
candidate remains read-only during rendering.

## Live authentication

The two live checks establish that the tested credentials were forwarded and the
requested task completed. Codex's model identity comes from its CLI session record;
Claude's comes from its response stream. These records are not independent proof of
provider-side routing. Each check ran once, sequentially, with a five-minute agent
timeout. Saved outputs were scanned for the supplied credentials; no matches were found,
and no files exceeded the scanner's size limit. The host Codex auth file was unchanged.
All test outputs were kept outside the release directory.

## Bounded live campaigns

The RQ1 check used Codex `gpt-5.6-sol` with `max` reasoning and the Tracing card. It
completed one episode in 15.7 minutes of agent time, or 16.7 minutes including setup and
verification. One world passed the protected checks and was retained. The controller
stopped with 4.3 minutes unused because another episode requires at least five minutes.
No infrastructure failure was recorded.

The RQ2 check used Claude Code `claude-opus-5` with `high` reasoning and the Measurement
card. Both episodes reached their CLI time limits. The first submission passed protected
checks but lacked required completion records, so it was preserved without acceptance.
The second had complete records, passed verification, and was retained. Its measured
support category matched the request (`focal`, `multipart`). Both episodes passed all
four isolation checks and all five verifier checks.

RQ2 used 30 minutes 9.5 seconds of recorded agent-execution time and 33.2 minutes
including setup and verification. The 9.5-second excess over the configured agent budget
includes timeout handling; this is a wall-time limit, not an exact cutoff of provider
compute. The controller exited normally with no infrastructure failure.

At most two coding agents ran concurrently. Saved campaign files were scanned for
supplied credential material without a file-size exclusion; no matches were found. The
test containers were removed. Run outputs remain outside the release repository.

These runs check generation, submission, protected verification, and controller
behavior. Passing does not establish human-rated question quality, profile adherence, or
reproducibility of the paper's discovery counts.

## Checks that remain

Live OpenCode authentication, RQ3 evaluator API access, six-hour campaigns, and live
coverage of all nine profiles remain untested. Linux x86_64 and WSL have not been tested
locally. The RQ3 Docker check covers its starting inputs and protected verification; its
feedback-processing tests use simulated responses rather than live evaluators.

The Docker smoke test checks execution and isolation with a fixed solution. It provides
no evidence about the quality of newly generated worlds.

## Repeat the checks

```bash
uv sync --locked --extra runner --group dev
uv run --extra runner pytest -q
uv run ruff check scripts src tests worlds harbor
uv run ruff format --check scripts src tests worlds harbor
bash harbor/build_latex_tikz_runtime.sh
uv run --extra runner python -m scripts.doctor
uv run --extra runner python -m scripts.docker_smoke --rq 1 --out artifacts/rq1-smoke
uv run --extra runner python -m scripts.docker_smoke --rq 2 --out artifacts/rq2-smoke
uv run --extra runner python -m scripts.docker_smoke --rq 3 --out artifacts/rq3-smoke
uv run --extra runner python -m scripts.check_agent_installations
uv run --extra runner python -m scripts.check_starting_collections
uv run --extra runner python -m scripts.audit_release
uv run --extra runner python -m scripts.package_release
```

Use new output paths when repeating checks. Diagnostics are saved under ignored
`artifacts/`. The source ZIP excludes those outputs, Git history, virtual environments,
and local campaign configurations. It includes a SHA-256 manifest.

## Known small-sample failure

With ten scenes and random seed 101, `stitch_face_alternation` produced three answer
classes; its verifier requires at least four. That check therefore failed. The other 19
worlds passed. All 20 worlds passed with 32 scenes.

The standalone checker defaults to 32 scenes. The RQ3 feedback policy still uses ten
scenes, and no verifier threshold was changed. Starting collections load fixed feedback,
so campaign initialization does not run this evaluation again. To reproduce the
ten-scene failure:

```bash
uv run --extra runner python -m scripts.check_starting_collections \
  --only stitch_face_alternation --scenes 10 --out artifacts/stitch-ten-scene-check
```
