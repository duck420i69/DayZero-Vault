# DayZero Vault — Context

Offline-first private Windows desktop workspace: one milestone countdown, a daily English quote, and an encrypted journal with full Vietnamese input. Built as a fork of Fenmark (formerly Mini Diarium). Decisions live in `docs/adr/`.

## Glossary

- **Vault** — the encrypted journal database plus its auth slots. Locked by default; nothing in the app is visible until it is unlocked.
- **Master key** — random 256-bit key that encrypts vault data. Never derived from a password; only stored wrapped inside auth slots.
- **Auth slot** — one wrapped copy of the master key, unlocked by one method: master password (Argon2id), recovery code, key file, or Windows Hello (per PC).
- **Recovery code** — one-time code generated at vault setup and shown once; it holds its own auth slot. Losing the password and the recovery code means the data is lost.
- **Countdown** — the single active milestone: title, start date, target date. Progress = elapsed / (target − start). Older countdowns can be archived.
- **Micro-milestone** — a named checkpoint on the countdown (for example "Halfway", "Under 200 Days", "Final 30 Days").
- **Daily Vibe** — the quote module under the countdown. One quote per day per mode, chosen from the system date; Shuffle overrides it.
- **Zen mode / Unfiltered mode** — Daily Vibe modes: calm stoic quotes, or blunt venting quotes (`zen` and `vent` arrays).
- **Built-in quotes** — shipped `quotes.json`, plaintext, hand-editable.
- **Custom quotes** — quotes the user adds; stored encrypted in the vault.
- **Entry** — one dated journal record, encrypted, optionally tagged with a mood.
- **Vent & Shred** — scratchpad held only in memory, never written to disk; one click wipes it.
- **Upstream** — github.com/fjrevoredo/mini-diarium (Fenmark), MIT, Tauri 2 + Rust + SolidJS.

## MVP scope

In: Countdown + Daily Vibe (home view), Journal + Vent & Shred (journal tab, single journal), moods, recovery code, Windows Hello slot, portable build.
Deferred: panic/boss key, minimalist mode, encrypted-attachment extras beyond upstream.

## Known risks

- Vietnamese IME (Telex/VNI, UniKey, EVKey) inside TipTap/ProseMirror on WebView2: test in the first build, before feature work.
- Windows Hello slot ties to one PC's TPM; password and recovery code remain the portable unlock paths.
