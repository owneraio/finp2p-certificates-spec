# Agreement definitions — demo

An agreement **definition** is the state machine co-signed alongside an
agreement's terms. It declares the states an agreement may occupy, which event
types may be appended, the guard each event must satisfy to be admitted, and the
transitions that move the machine. It is part of the signed contract zone:
authored once when the agreement is proposed, and an amendment cannot change it.

## collateral-facade-v1.definition.json

The machine the collateral facade attaches when an agreement is proposed with
`definitionTemplate: collateral-facade-v1`.

**This file is GENERATED** from the shipped Go template
(`finp2p-core/apis/collateral/internal/command/definition.go`) and must not be
hand-edited — a test in that package compares the two and fails on drift. What a
router attaches is the Go value; this file is the published, readable copy.

Two things about it are worth knowing:

- **Guards read protocol fields only** — `businessState`, `emitterRole` and
  event lineage. None reads a payload, because a payload is opaque bytes to a
  guard. That is why the terminal `report-status` meanings are separate event
  types rather than one type discriminated by a payload field.
- **Every event type references a payload schema by url**, pinned to a commit.
  The router fetches each document when the agreement is proposed, writes its
  sha256 digest into the definition, and every party re-verifies those digests
  before it signs — so the parties consent to schema *content*, not to a pointer
  that could later serve something else. The `hash` fields are therefore absent
  here and present on a stored agreement.

The referenced schemas live in [../../schemas/agreements/demo](../../schemas/agreements/demo).
