# Checkpoint–Measurement relationship mismatch

**Status:** Temporary internal note  
**Recorded:** 2026-08-12

## Intended model

A Measurement is a reusable evaluation contract. It does not execute work.
An eval Run applies one Measurement to one model or Checkpoint and records the
result.

The intended relationship is:

```text
Checkpoint + Measurement -> eval Run -> score and Traces
```

A model or Checkpoint should be evaluable under more than one Measurement.
Each pairing produces a separate eval Run. Each Measurement keeps its own
Baseline and computes deltas only within its own Cohort.

This makes Checkpoints and Measurements many-to-many through eval Runs:

```text
                       Measurement A -> eval Run A -> result A
One Checkpoint ------> Measurement B -> eval Run B -> result B
                       Measurement C -> eval Run C -> result C
```

A training Run is different. It applies a Recipe to a base model or parent
Checkpoint and produces a new Checkpoint. Follow-up eval Runs establish whether
that Checkpoint improved under each relevant Measurement.

## Current platform behavior

The current platform is Measurement-centric:

- Every Run belongs to one Measurement's Ledger.
- A training Run produces a Checkpoint inside that Measurement.
- The next training Run continues the newest admissible Checkpoint in the
  Cohort.
- A produced Checkpoint cannot be evaluated or used as a parent in another
  Measurement.
- Base models may appear in several Measurements, but produced Checkpoints may
  not cross the Measurement boundary.

This prevents the intended workflow: train a model once, then evaluate the same
Checkpoint under several frozen evaluation contracts.

## Why Measurement became the root

The v1 design began with a narrower objective: one particular problem goes in,
the loop produces a measurably better model or an honest null result, and one
Ledger records the work. The original `E2E-GRILL.md` called this the
per-Problem Ledger. The later vocabulary change renamed that frozen problem
definition to Measurement.

The Prime Intellect research then added the part Prime lacked: a model-free,
frozen comparison boundary. Models would bind per Run, while the Measurement
would freeze the Splits, eval settings, and Environment fingerprint. This made
the Measurement the natural namespace for:

- `monte eval <measurement>` and `monte train <measurement>`;
- `<measurement>/<run>` addresses;
- the Baseline gate and comparability checks;
- Split ownership;
- the append-only Ledger and its on-disk directory.

The v1 plan therefore says that a Measurement goes in and a promoted Checkpoint
plus an honest Ledger comes out. The hierarchy in `CONTEXT.md` and the current
introduction follow that implementation boundary.

This was a sound shape for proving one end-to-end loop. It did not settle the
broader product relationship between reusable Checkpoints and multiple
Measurements. The current cross-Measurement restriction is a consequence of
the v1 namespace and ownership model, not an explicitly defended product
requirement.

## Documentation decision

The introduction should not put Measurement at the top of the operational
diagram. It should show Measurement as one input to an eval Run:

```text
Environment -> Measurement
                     \
Checkpoint ----------> eval Run -> result
```

The training path should remain separate:

```text
Parent Checkpoint + Recipe -> training Run -> new Checkpoint
```

The public docs must distinguish current behavior from the intended
many-to-many model until cross-Measurement Checkpoint references are
implemented. Do not present cross-Measurement evaluation as available today.

Updating the introduction's diagram is a documentation correction. Enabling a
produced Checkpoint to cross Measurement boundaries is a platform data-model
change. It affects Checkpoint identity and lookup, Run plans, CLI addressing,
Ledger ownership, Cohort and Lineage reads, and artifact provenance. It should
be designed separately rather than implied by the diagram.

## Platform questions to settle later

1. Give every Checkpoint an identity that can be referenced outside its
   producing Measurement.
2. Decide whether training Runs remain owned by one Measurement or move to a
   higher-level Ledger while Measurements own only eval Runs.
3. Define how an eval Run records a Checkpoint produced by another Measurement
   without duplicating its provenance.
4. Keep Baselines and deltas scoped to one Measurement and Cohort.
5. Decide which Measurement or Environment supplies training reward when a
   Checkpoint will be evaluated under several Measurements.
6. Decide whether a later Experiment or Suite groups this work. Experiment is
   reserved and not implemented, so do not use it to patch the current model.
