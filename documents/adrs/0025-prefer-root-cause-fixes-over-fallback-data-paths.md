# ADR-0025: Prefer root-cause fixes over fallback data paths

- Status: Accepted
- Date: 2026-06-25

## Context

When a persisted document fails to load, several distinct failure modes can look similar from the UI:

- the data may be genuinely absent
- the data may exist but fail validation
- the data may exist and be valid, but a normalization step may corrupt it before validation
- the error message may describe the wrong cause

If these cases are collapsed into a single "missing data" response, debugging becomes slower and misleading. Developers may add fallback writes, default documents, or self-healing seed paths to make the screen load again, but those measures can hide the real defect and leave the underlying contract mismatch in place.

## Decision

Data-loading and provisioning code follows four rules:

1. Loader errors must reflect the real failure mode.
If persisted data is absent, the error may say it is unprovisioned. If persisted data exists but cannot be validated or normalized, the error path must preserve that distinction for debugging and repair.

2. Normalization must preserve values that are valid under the schema contract.
Normalization may trim, reorder, or fill optional derived fields, but it must not coerce an allowed value into an invalid one.

3. Provisioning happens through the authoritative provisioning path only.
If the system is designed to provision baseline data through database migration, seed, or another explicit bootstrap step, runtime loaders must not silently create replacement records to mask defects.

4. Fix the root cause before adding fallback behavior.
Fallback reads or writes are acceptable only when they are an intentional part of the product contract. They must not be used as a shortcut around an unresolved validation, normalization, or migration defect.

## Consequences

Failures should be easier to diagnose because the code distinguishes between missing data, invalid data, and corrupted read paths.

Provisioning remains predictable. Developers can trust that seeded or migrated content is the source of truth, rather than wondering whether runtime code has created substitute records behind the scenes.

This decision raises the bar for "temporary" recovery logic. Before adding a fallback, a developer must first prove that the problem is genuine absence rather than a broken read or transformation path.

## Rejected alternatives

Treating every load failure as "data not provisioned" was rejected because it hides contract bugs behind a misleading message.

Adding runtime self-healing or safety-seed behavior by default was rejected because it can mask broken validation or normalization logic and make future defects harder to detect.

Allowing normalization layers to reshape data aggressively was rejected because boundary code should preserve the schema contract, not redefine it implicitly.

## Related decisions

- [ADR-0012: Pass undefined to img src instead of empty string](./0012-guard-image-src-attributes-against-empty-strings.md) — rendering guards belong at the UI boundary and should not be confused with persistence rules.
- [ADR-0024: Maintain zero TypeScript diagnostics](./0024-maintain-zero-typescript-diagnostics.md) — both decisions reinforce that code quality gates should surface real contract mismatches instead of tolerating them.
