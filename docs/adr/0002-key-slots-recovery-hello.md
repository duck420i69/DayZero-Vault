# 0002. Key slots with recovery code and per-PC Windows Hello

Date: 2026-11-22 · Status: Accepted

## Decision

A random master key encrypts all vault data. Each unlock method holds a wrapped copy in its own auth slot (upstream `auth_slots` model):

- Master password (Argon2id + AES-256-GCM) — always present, works on any PC.
- Recovery code — generated once at setup, shown once.
- Windows Hello — optional, enrolled per PC. Its slot is bound to that PC's TPM, so it never replaces the password and does not break USB portability.

## Consequences

- Password change rewraps one slot; no re-encryption of data.
- Upstream has no recovery code ("There is no password recovery") and no Hello; both are new work.
- Losing password and recovery code loses the data.
