# Release scope and differences

This release runs the three discovery experiments. It includes the controller, three
coding-agent interfaces, public and protected checks, three shared examples, nine
semantic profiles, and two ten-world starting collections. RQ4 training, historical
campaign outputs, and the paper's separate 200-scene replay analysis are not included.

## Versions

The release uses `question-world` 0.4, `episodic-sequential` 0.7, `baseline_three` 0.2,
`paper-semantic` 0.3, `quality-diversity` 0.8, and `difficulty-feedback` 0.9. Some
historical RQ3 campaigns used `episodic-sequential` 0.8. That variant is not included.

There is one example configuration for each of Codex, Claude Code, and OpenCode. The
paper uses five model/reasoning configurations. Reproducing the complete experiment
matrix requires those configurations and model access. Available models and their
generated outputs may differ from the original runs.

## Changes for this release

- The prepare, preview, freeze, and run commands provide a common interface to the
  three coding agents. Credentials come from environment variables or files
  supplied by the user.
- Package and container names use neutral names. Original Git history,
  author identities, personal paths, and personal remotes were not copied.
- Candidate tests run in a temporary copy because some write `self_check.json`.
  The original source remains mounted read-only. Rendering uses that source in a
  separate offline container. No candidate test is skipped.
- Manuscript sources, historical agent traces, rendered galleries, and author
  review files are excluded. The full Figure 1 diagram is included for the README.

The world algorithms, answer classes, inverse tolerances, protected checks, and
image-support thresholds are unchanged.

Five source labels in the semantic profile cards use a neutral benchmark name in this
release. The question directions and construction instructions are unchanged. This label
substitution is an anonymization change to the supplied text; these cards are not
byte-identical copies of the historical inputs.

## RQ3 starting inputs

Starting collections retain world source, tests, candidate metadata, and deviation
records. Their feedback includes aggregate accuracies, question text, answer choices,
gold answers, and predictions. These fields are visible to the coding agent.

Previously rendered images and their accompanying records are excluded. This includes
collection A's 32-record `run_bar_decompression` manifest and its HTML gallery, so the
release provides fewer starting scene specifications and answers than that historical
input bundle. World logic and retained feedback values are unchanged. Content hashes
were recomputed for the release files.

Historical bookkeeping was removed from feedback summaries and manifests. Image paths in
the sample records are references to the original evaluation; the image files are
absent. New candidate evaluations render images locally. Starting collections load their
fixed feedback without rerunning evaluation at initialization.

## Known inconsistencies in frozen instructions

The instruction files preserve the experimental content, with the source-label
substitution noted above. Some wording is inconsistent with the files actually supplied:

| Frozen wording | Actual input |
| --- | --- |
| Semantic cards say “Author choice pending” | The release uses the fixed `paper-semantic` 0.3 cards |
| Some RQ3 instructions say “five conceptual seeds” | Each starting collection contains ten worlds |
| Feedback prose says gold labels are hidden | Starting-feedback JSON includes gold answers |

Use [the input guide](INPUTS.md) for the supplied files and what agents can read.
Changing the frozen prompts would require a new experimental version. The input guide
also explains older terms such as “working seed,” “inverse arm,” and “survivor commit,”
and identifies references to development documents that are not packaged.

## Local outputs and packaging

Campaign outputs and virtual environments are excluded from the source release.
`scripts.package_release` produces a ZIP from the approved source and input files, with
no Git history or run logs. It does not back up local campaigns.

The release scan checks file names, content patterns, and the approved file list. It
cannot guarantee anonymity against external knowledge or source-code matching. See
[validation](VALIDATION.md) for the checks performed and the known ten-scene
answer-class coverage failure in `stitch_face_alternation`.
