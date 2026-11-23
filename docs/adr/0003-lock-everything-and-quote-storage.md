# 0003. Lock everything; split quote storage

Date: 2026-11-22 · Status: Accepted

## Decision

- The app opens to the unlock screen. Countdown, quotes, and journal stay hidden until unlock, because the app is meant for shared PCs.
- Built-in quotes ship as plaintext `quotes.json` (`zen` and `vent` arrays). Custom quotes the user adds are stored encrypted in the vault.

## Consequences

- No glance-without-password view.
- Countdown title and dates must live in the vault, not in plaintext preferences.
