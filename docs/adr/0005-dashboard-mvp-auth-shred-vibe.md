# 0005. Dashboard placement, MVP auth, Vent & Shred, Daily Vibe defaults

Date: 2026-11-22 · Status: Accepted · Revised 2026-11-23 after council review

## Decisions

1. **Dashboard** — after unlock the app lands on a home view (countdown + Daily Vibe); the journal is a tab. One journal only: upstream multi-journal UI is hidden. The countdown is stored encrypted in the vault, never in upstream preferences (they live in plaintext WebView localStorage).
2. **MVP auth** — master password plus recovery code (design in ADR 0007). Windows Hello and the portable build move to v2.
3. **Vent & Shred** — memory only, never written to disk. Wiped on lock, idle-lock, and close. Spellcheck disabled in the scratchpad. A "Save to journal" button turns the current vent into an ordinary encrypted entry, then wipes the pad.
4. **Daily Vibe** — Zen by default; the last chosen mode is remembered. Daily quote index = hash(local date) per mode. Shuffle overrides until local midnight. MVP uses built-in quotes only; custom quotes move to v2.

## Deferred to v2

- **Windows Hello slot.** Windows Hello (KeyCredentialManager) only signs data and gives no secret that can wrap the master key. A safe slot needs a deterministic signature over a fixed challenge fed to a KDF, plus a check that the key is TPM-backed. The easy shortcut (UserConsentVerifier plus a DPAPI-stored key) is only a UI gate and fails the shared-PC threat in ADR 0003.
- **Portable build** (`./data` next to the exe). When built: no DLL loading from the exe folder; WebView2 user-data and preferences move with the data.
- **Custom quotes** (stored encrypted in the vault, per ADR 0003).

## Consequences

- JS strings cannot be reliably zeroed; "wipe" means clearing the editor state and dropping references, and pad text is never sent to the Rust side.
- Last Vibe mode can sit in plaintext preferences; it reveals nothing private.
