# Records of known and rejected mechanisms

The controller and protected verifier use these fixed JSONL tables to compare new worlds
with known tasks and earlier rejected proposals. They are part of the experiment inputs:
removing entries changes discovery and acceptance decisions.

The tables contain task descriptions, mechanism signatures, and outcomes. They do not
contain agent transcripts or rendered galleries. Historical task IDs remain so
references between records can be followed. New campaigns write their own state; they do
not update these tables.
