# 0006. Milestones, Day Zero behaviour, repo setup, toolchain

Date: 2026-11-22 · Status: Accepted

## Decisions

1. **Micro-milestones** — ship defaults (Halfway, Under 200 Days, Under 100 Days, Final 30 Days, Final 7 Days); the user can add and remove them. Stored encrypted with the countdown.
2. **Day Zero** — when the target is reached, show "Day Zero reached", then count up ("X days since") and prompt to archive the countdown and set a new one.
3. **Repo** — user forks fjrevoredo/mini-diarium on GitHub; clone the fork into this folder, add `upstream` remote, move these docs into it.
4. **Toolchain** (checked 2026-11-22) — present: Rust 1.99.1 (rustup), Node 24.4.1, Visual Studio Community 2022 with C++ tools, WebView2 runtime. Missing: Bun, which upstream uses as its JS package manager.

## Consequences

- Bun must be installed (or upstream scripts run via npm) before the first build.
