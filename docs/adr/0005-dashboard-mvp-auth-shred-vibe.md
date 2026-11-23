# 0005. Dashboard placement, MVP auth, Vent & Shred, Daily Vibe defaults

Date: 2026-11-22 · Status: Accepted

## Decisions

1. **Dashboard** — after unlock the app lands on a home view (countdown + Daily Vibe); the journal is a tab. One journal only: upstream multi-journal UI is hidden. The countdown is stored in that vault.
2. **MVP auth** — recovery code, Windows Hello slot, and the portable build are all in the MVP.
3. **Vent & Shred** — wiped on lock, idle-lock, and close. Spellcheck disabled in the scratchpad so no words reach a dictionary file. A "Save to journal" button turns the current vent into an encrypted entry, then wipes the pad.
4. **Daily Vibe** — Zen by default; the last chosen mode is remembered. Daily quote index = hash(local date) per mode. Shuffle overrides until local midnight.

## Consequences

- MVP is larger: Hello needs Windows-only Rust code (`windows` crate, KeyCredentialManager / Windows Hello APIs) behind a cfg gate.
- Last Vibe mode can sit in plaintext preferences; it reveals nothing private.
- JS strings cannot be reliably zeroed; "wipe" means clearing the editor state and dropping references, and the pad text is never sent to the Rust side.
