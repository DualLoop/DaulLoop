# RQ4 — Downstream adaptation: release scope

The current paper describes supervised fine-tuning in **§4.7**, with results in
**§5.5**. It tests whether data from generated worlds improves open-weight models on
held-out worlds and external benchmarks.

This repository releases the three discovery loops (RQ1–RQ3). It does **not** include
RQ4's training data, dataset splits, LoRA training pipeline, checkpoints, or code for
evaluating external benchmarks. There is therefore no RQ4 run script. The generation
examples elsewhere in this folder do not reproduce that experiment.

The three executable reference worlds can demonstrate image and question generation
locally, but they are not the paper's fine-tuning dataset. See the [shared-example
walkthrough](../shared_examples/README.md).
