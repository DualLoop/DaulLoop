# RQ3 starting collections

Collections A and B each contain ten world implementations and fixed evaluator feedback.
Each manifest records SHA-256 hashes for the candidate code, sampled instances, and
feedback files. The controller checks these before preparing a run.

Historical rendered evidence and source bookkeeping are excluded, so release hashes
differ from the original campaign bundles. World answer logic and inverse algorithms are
unchanged. Feedback JSON retains gold answers, which are visible to the agent. See
[release differences](../../docs/RELEASE_NOTES.md) and the [RQ3
walkthrough](../../examples/rq3/README.md).

The candidate `deviations.md` files are preserved authoring records. Some mention
selection notes or galleries omitted from the release. These references do not indicate
additional files needed to run the supplied implementations.
