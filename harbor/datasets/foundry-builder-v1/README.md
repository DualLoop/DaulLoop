# Shared question-world task template

This Harbor template supports both multi-candidate tasks and single-candidate trials. Do
not launch an agent directly against the template. The preparation code combines it with
a versioned protocol, shared examples, task limits, and container settings, and records
hashes of the resulting files. The release uses single-candidate episodes; follow the
[walkthroughs](../../../examples/README.md).

The bundled reference-marker solution is an executable world used to test packaging and
protected verification. It is supplied only during fixed-example smoke tests, not during
normal discovery.
