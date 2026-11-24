# 0004. Encryption layering, fork style, portable mode, moods

Date: 2026-11-22 · Status: Accepted · Revised 2026-11-23 after council review

## Decisions

1. **Encryption** — keep upstream app-level AES-256-GCM; no SQLCipher. Upstream already encrypts tag names (`tags.name_encrypted`, `crates/mini-diarium-core/src/db/schema/create.rs:179`), so no tag work is needed. DayZero data (countdown, moods) goes only into encrypted storage, never into plaintext columns or preferences. Entry dates stay visible on disk (accepted leak).
2. **Fork style** — downstream fork. Upstream code stays mostly untouched; new features live in their own Rust module and SolidJS routes. UI changes to upstream's shell (home view, hidden multi-journal) go through one small, documented route seam so merges conflict in one place. Merge tagged upstream releases, reviewing each diff.
3. **Portable mode** — deferred to v2 (see ADR 0005). The app ID still changes from `com.minidiarium` in the first code commit, so the fork never shares data with an installed Fenmark and no later data migration is needed.
4. **Moods** — fixed small mood set, stored under a namespaced key (`dz.mood`) inside the existing per-entry `entry_metadata_encrypted` JSON (`create.rs:150`). No schema change; upstream backup and restore already round-trip that column.

## Consequences

- SQLCipher rejected: auth slots would need a sidecar file, and the fork diff and merge cost would grow.
- The MVP needs no schema change. Table rules for later additions are in ADR 0007.
- Dissent kept: the council's devil's advocate argued for a hard fork (pin one release, cherry-pick security fixes only) because shell UI edits will conflict on every merge. Revisit if merges prove costly.
