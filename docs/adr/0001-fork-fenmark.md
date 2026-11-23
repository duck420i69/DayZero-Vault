# 0001. Fork Fenmark instead of building from scratch

Date: 2026-11-22 · Status: Accepted

## Context

The proposal asked for C# .NET 8 + WPF + SQLCipher. .NET 8 left support on 10 Nov 2026. Fenmark (formerly Mini Diarium, MIT, v0.8.1 on 18 Nov 2026) already ships an offline encrypted journal: AES-256-GCM entries in SQLite, Argon2id-wrapped master-key slots, key-file unlock, idle and focus-loss auto-lock, in-memory decrypting search, tags, attachments, backups, no HTTP client.

## Decision

Fork Fenmark. Stack becomes Tauri 2 (Rust core, WebView2) + SolidJS + TipTap. Add countdown, Daily Vibe, Vent & Shred, recovery code, Windows Hello slot, and portable mode on top.

## Consequences

- Journal and crypto core come proven; work focuses on new features.
- Not C#/WPF. UI is web tech inside WebView2; Vietnamese IME (Telex/VNI, UniKey, EVKey) in TipTap/ProseMirror must be tested early.
- Must keep upstream MIT notice. Upstream docs worth reading: `docs/ARCHITECTURE.md`, `SECURITY.md`, `AGENTS.md`, `crates/mini-diarium-core/API.md`.
- Upstream has no portable exe (installers default to `%APPDATA%\com.minidiarium\`) and keeps preferences in unencrypted WebView localStorage; portable mode must move both.
