# What the coding agent receives

| Input | RQ1 | RQ2 | RQ3 |
| --- | --- | --- | --- |
| Shared code examples | Three, identical | Same three | Same three |
| Nine semantic profiles | One selected card | One selected card | None |
| Five short question directions | All five in that card | All five in that card | Uses its ten-world starting card instead |
| Ten starting implementations | No | No | Collection A or B |
| Image-support targets | No | Adaptive request in text and JSON | No |
| Evaluator feedback | No | No | Starting summaries, then summaries of new evaluations |
| Accepted world code | Grows as worlds are retained | Grows as worlds are retained | Starts with ten worlds; later retained worlds are added |
| Most recent rejected code and report | Available after rejection | Available after rejection | Available after rejection |
| Public checks and submission requirements | Yes | Yes | Yes |

`instruction.md` directs the agent to read files in `/workspace/`. Profile question
sentences are supplied in `DISCOVERY_PROFILE.md`; the initial instruction directs the
agent to that file. All five remain fixed for that profile across episodes. Each card
also specifies the target computation, guidance for construction and inverse solving,
validity requirements, and rules against superficial copies of existing tasks. A source
ID such as `blind_network_farthest` records where a direction came from. It does not
load task code or an image. No corresponding profile-specific implementation or reader
illustration is supplied.

The shared examples are `two_circles`, `angle_acuteness`, and
`counting_with_distractors`. The shared-example configuration supplies only `DESIGN.md`,
`world.toml`, `renderer.py`, `prompts.py`, and `oracle.py` for each. These source
snapshots show how to sample and render scenes, map questions to answers, and recover
answers independently from pixels. Answer computation may be defined in these modules
rather than in a separate forward-program file. The release also includes runnable
generators for users; those entry points are not added to the builder's five-file
snapshots.

Profile cards do not define complete instances. For example, “Which terminal is farthest
from green along the lines?” is one direction; the agent supplies a complete public
question specification, randomized scene generator, and inverse solver for its new
world.

Generation, repair, submission and verification behavior comes from separate versioned
strategy instructions and public tools, not from the three shared code examples.
Protected test files are excluded from the agent's starting directory and uploaded to a
clean container after submission. Their source is included in this repository, so anyone
reading the repository can inspect it. The restriction applies to the files delivered
during an agent episode.

RQ3 feedback JSON includes answers and correctness for starting worlds and earlier
candidates. Visibility descriptions in the profile metadata apply only to the card; they
do not describe the separate starting collection and feedback files. Protected
verification of a new submission remains a separate stage. Availability of any file does
not guarantee a future agent will read it; inspect the local run's saved agent
transcript to see which files it read. No historical trajectories are included.

## Terms used in the preserved instructions

Some files retain terminology from the original implementation. The terms below refer to
different parts of the workflow.

| Term in source files | Meaning |
| --- | --- |
| Builder, inner builder | The coding agent that implements one new world |
| Inverse arm, pixel arm, oracle | The program that recovers an answer from the rendered image |
| Analytic gold | The expected answer computed from the scene specification |
| Working seed | The collection of accepted worlds supplied to later episodes |
| Seed snapshot, seed set | The three shared implementation examples; RQ3 also has a separate starting collection |
| Materialized packet | The task files assembled for one agent episode |
| Gate | A check that a submission must pass |
| Survivor commit, envelope check | The recorded completion of a candidate submission; this is not a Git commit |
| Canonical admission | Human approval for inclusion in the research world bank; the release controller does not perform it |
| Descriptor cell, elite archive | An image-support category and the retained representatives of those categories in RQ2 |
| Random seed | A number or string used to make sampling reproducible |

The original `DESIGN.md` files supplied with the shared examples include references to
an older SSOT (single source of truth), `docs/verification-boundary.md`, and calibration
scripts that are not packaged here. Those references describe the examples' development
history. Use the [shared-example walkthrough](../examples/shared_examples/README.md) for
supported release commands and the versioned contracts under `foundry/` for new-world
requirements.
