# 0007. Recovery code, threat model, and fork rules

Date: 2026-11-23 · Status: Accepted (from council review of the plan)

## Decisions

1. **Recovery code** — a printable encoding of upstream's existing key-file (`keypair`, X25519) slot: random, 256 bits, never derived from the password. It reuses upstream slot wrapping, adds no new KDF or primitive, and adds no new slot type, so `auth_core.rs`, `auth_policy.rs` and `auth_slots.rs` stay untouched. Setup shows the code once and makes the user confirm it was saved (for example by re-typing part of it). After the code is used to unlock, the app prompts to rotate it.
2. **Untrusted local files** — `quotes.json` and plaintext preferences sit where other users of a shared PC or USB stick can edit them. They load through one validating module that checks the schema; quotes render as plain text only, never HTML.
3. **Threat model** — write `docs/THREAT_MODEL.md` before Vent & Shred ships. In scope: shared-PC onlookers, stolen USB/disk copies, edited local files. Out of scope, as in upstream `SECURITY.md`: malware and keyloggers on an unlocked session. Known leak paths to document and reduce: IME user dictionaries (UniKey/EVKey), the clipboard, and the WebView2 profile folder.
4. **Fork data rules** — never change upstream's `SCHEMA_VERSION` (14 today, `crates/mini-diarium-core/src/db/schema/mod.rs:94`). Any table the fork adds uses a `dz_` prefix, stores only ciphertext for private data, and has its own version key. Keys the fork adds inside upstream JSON blobs use a `dz.` prefix.
5. **Order of work** — (a) install Bun, build plain upstream, and run the Vietnamese IME test (Telex, VNI, UniKey, EVKey) as a go/no-go gate; (b) first code commit changes the app ID from `com.minidiarium`; (c) features.
6. **Supply chain** — commit lockfiles, install frozen, and review each upstream merge diff, especially `Cargo.lock` and `crates/`. Upstream `SECURITY.md` is stale (lists 0.5.x as supported).

## Consequences

- MVP needs no new auth code paths beyond setup UI and code encoding.
- If the IME test fails, ADR 0001 (fork Fenmark) is reopened before any feature work.
