# DayZero Vault — Context

Offline-first private Windows desktop workspace: one milestone countdown, a daily English quote, and an encrypted journal with full Vietnamese input. Built as a fork of Fenmark (formerly Mini Diarium). Decisions live in `docs/adr/`.

## Glossary

- **Vault** — the encrypted journal database plus its auth slots. Locked by default; nothing in the app is visible until it is unlocked.
- **Master key** — random 256-bit key that encrypts vault data. Never derived from a password; only stored wrapped inside auth slots.
- **Auth slot** — one wrapped copy of the master key, unlocked by one method: master password (Argon2id), recovery code, key file, or (v2) Windows Hello per PC.
- **Recovery code** — printable encoding of a random 256-bit key-file slot, generated at vault setup and shown once. Losing the password and the recovery code means the data is lost.
- **Countdown** — the single active milestone: title, start date, target date. Progress = elapsed / (target − start). Older countdowns can be archived.
- **Micro-milestone** — a named checkpoint on the countdown (for example "Halfway", "Under 200 Days", "Final 30 Days").
- **Daily Vibe** — the quote module under the countdown. One quote per day per mode, chosen from the system date; Shuffle overrides it.
- **Zen mode / Unfiltered mode** — Daily Vibe modes: calm stoic quotes, or blunt venting quotes (`zen` and `vent` arrays).
- **Built-in quotes** — shipped `quotes.json`, plaintext, hand-editable; treated as untrusted input and rendered as text only.
- **Custom quotes** (v2) — quotes the user adds; stored encrypted in the vault.
- **Entry** — one dated journal record, encrypted, optionally tagged with a mood (stored as `dz.mood` in the entry's encrypted metadata).
- **Vent & Shred** — scratchpad held only in memory, never written to disk; one click wipes it.
- **Upstream** — github.com/fjrevoredo/mini-diarium (Fenmark), MIT, Tauri 2 + Rust + SolidJS.

## MVP scope

Gate: Vietnamese IME test on a plain upstream build passes before any feature work (ADR 0007).
In: Countdown with fixed micro-milestones + Daily Vibe with built-in quotes (home view), Journal with moods + Vent & Shred (journal tab, single journal), password + recovery code.
v2: Windows Hello slot, portable build, custom quotes, editable micro-milestones.
Later: panic/boss key, minimalist mode, encrypted-attachment extras beyond upstream.

## Success measures

- Used daily for 30 days.
- Zero Vietnamese input bugs (Telex, VNI, UniKey, EVKey).
- Setup to first journal entry in under 2 minutes.

## Known risks

- Vietnamese IME inside TipTap/ProseMirror on WebView2 could fail; that reopens ADR 0001.
- Upstream shell UI edits (home view, hidden multi-journal) will conflict on merges; keep them behind one route seam (ADR 0004).
- Vent text can leak via IME dictionaries, clipboard, or the WebView2 profile; see the threat model (ADR 0007).
