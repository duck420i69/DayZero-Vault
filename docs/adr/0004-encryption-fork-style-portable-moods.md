# 0004. Encryption layering, fork style, portable mode, moods

Date: 2026-11-22 · Status: Accepted

## Decisions

1. **Encryption** — keep upstream app-level AES-256-GCM; no SQLCipher. Additionally encrypt fields upstream leaves plain where DayZero adds or relies on them: tags, moods, countdown, custom quotes. Entry dates stay visible on disk (accepted leak).
2. **Fork style** — downstream fork. Upstream code stays mostly untouched; new features live in their own Rust module and SolidJS routes. Merge upstream releases regularly to keep security fixes.
3. **Portable mode** — two builds. Installer build uses `%APPDATA%`; portable build switches to `./data` next to the exe (DB, WebView2 user-data folder, preferences) when a marker file is present. The app ID must differ from `com.minidiarium` so it never shares data with an installed Fenmark.
4. **Moods** — fixed small mood set, stored encrypted per entry, filterable after unlock.

## Consequences

- SQLCipher rejected: auth slots would need a sidecar file, and the fork diff and merge cost would grow.
- Encrypting tags changes an upstream table; keep that change small and documented for merges.
- Portable build still needs the WebView2 runtime on each PC (built into Windows 10/11).
