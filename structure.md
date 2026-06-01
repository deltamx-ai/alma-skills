# Alma — App Structure on This Machine

Alma is an **Electron-based desktop AI chat app** ("AI Provider Management Desktop App"). On this PC it's installed as a Debian package.

- **Package:** `alma` 0.0.756 (dpkg)
- **Binary:** `/usr/bin/alma` → `/etc/alternatives/alma` → `/opt/Alma/alma`
- **Launcher:** `/usr/share/applications/alma.desktop` (`StartupWMClass=Alma`)
- **Icon:** `/usr/share/icons/hicolor/512x512/apps/alma.png`
- **User CLI shim:** `/home/delta/.local/bin/alma`
- **Local REST API when running:** `http://localhost:23001` (spec at `~/.config/alma/api-spec.md`)

---

## 1. Installed Files — `/opt/Alma/` (read-only, from the .deb)

Standard Electron layout (Chromium + V8 + app code):

```
/opt/Alma/
├── alma                        # Electron main executable
├── chrome-sandbox, chrome_crashpad_handler
├── libEGL.so, libGLESv2.so, libffmpeg.so, libvulkan.so.1, libvk_swiftshader.so
├── icudtl.dat, resources.pak, *.pak, snapshot_blob.bin, v8_context_snapshot.bin
├── locales/                    # ~50 Chromium locale .pak files
├── LICENSES.chromium.html, LICENSE.electron.txt
└── resources/
    ├── app.asar                # packed JS app (main + renderer)
    ├── app.asar.unpacked/
    │   └── node_modules/       # native modules that can't live in asar
    │       ├── @anthropic-ai/          # Anthropic SDK
    │       ├── better-sqlite3/         # chat/thread DB
    │       ├── sqlite-vec, sqlite-vec-linux-x64/   # vector search
    │       ├── drizzle-orm/            # ORM over sqlite
    │       ├── node-pty/               # terminal/PTY for Bash-tool style use
    │       ├── onnxruntime-node, onnxruntime-web/  # local inference
    │       ├── @huggingface/, @img/, pdfjs-dist/, shiki/
    │       ├── electron-liquid-glass, jazz-midi, jieba-wasm, font-list, jszip
    │       ├── esbuild*, @esbuild, estree-util-to-js, @mdx-js/
    │       └── @napi-rs, @fugood, bufferutil, utf-8-validate
    ├── bundled-skills/         # 30 built-in skills (see §3)
    ├── cli/
    │   ├── alma                # Node script — CLI entry used by skills
    │   └── tui.mjs             # terminal UI
    ├── bun/bun                 # bundled Bun runtime (for skill scripts)
    ├── uv/                     # bundled uv (Python package runner, for tts etc.)
    ├── rtk/rtk                 # bundled Rust toolkit binary
    ├── tts/                    # Python TTS service
    │   ├── main.py
    │   └── requirements.txt
    ├── chrome-extension/       # companion browser extension
    │   ├── manifest.json, background.js, popup.{html,js}, options.{html,js}
    │   └── icons/
    ├── AppIcons/               # multi-platform icon sets (icns, ico, iconset, xcassets)
    ├── apparmor-profile
    ├── app-update.yml          # electron-updater config
    └── package-type
```

Takeaways:
- Electron shell hosting a Chromium renderer + Node main process.
- Ships its own **bun**, **uv**, and a **Rust binary (`rtk`)** so skills can run code without needing system installs.
- Chat/threads persisted in **SQLite with vector search** (`better-sqlite3` + `sqlite-vec`) via Drizzle ORM.
- Local model runtime via **onnxruntime** + `@huggingface` (explains the `embedding-models/` and `whisper_models/` in user data).
- Has an **Anthropic SDK** dep — Claude is a first-class provider.
- Bundles a **Chrome extension** for browser integration.

---

## 2. User Data — `~/.config/alma/` (Chromium profile + app state)

Alma uses a Chromium user-data directory (hence `Cookies`, `Local Storage`, `GPUCache`, `Preferences`, `Crashpad`, etc.), plus app-specific folders layered on top:

```
~/.config/alma/
├── (Chromium profile)   Cookies, Preferences, Local Storage/, Session Storage/,
│                        Cache/, Code Cache/, GPUCache/, DawnGraphiteCache/,
│                        DawnWebGPUCache/, Shared Dictionary/, SharedStorage,
│                        Dictionaries/, TransportSecurity, Trust Tokens, DIPS,
│                        Network Persistent State, VideoDecodeStats/, Crashpad/
│
├── chat_threads.db             # main SQLite DB (conversations) — ~12 MB here
├── chat_threads.db-shm, .db-wal
├── blob_storage/               # attachments / binary blobs
├── shared_proto_db/            # Chromium proto DB
│
├── workspaces/                 # per-workspace sandboxes (Alma's "rooms")
│   ├── default/home/           # default workspace, with a fake $HOME
│   └── temp-<id>/              # many ephemeral/scratch workspaces
│
├── skills/                     # user-installed skills (currently empty)
├── plugins/                    # user-installed plugins (empty)
├── plugin-cache/               # plugin runtime caches
├── plugin-storage/             # plugin per-install data
├── plugin-global-storage/      # plugin cross-install data
│
├── missions/                   # long-running "missions" (mntt…, mnwg…, …)
├── tasks/                      # persistent task logs
├── tasks.json                  # active task index (see live example below)
├── cron/
│   ├── jobs.json               # scheduled cron jobs
│   └── runs.json               # execution history
│
├── people/                     # contacts (e.g. deltamx.md + .avatar.discord.jpg)
├── groups/                     # group-chat state & logs
│   ├── discord-state.json,  discord_<channel>_<date>.log
│   ├── feishu-state.json,   feishu_<chat>_<date>.log
│   └── media/
│
├── selfies/                    # generated/captured self-images
├── gallery_cache/              # media thumbnails
├── embedding-models/           # downloaded embedding models
├── whisper_models/             # downloaded Whisper STT models
│
├── sentry/                     # Sentry crash/telemetry queue
├── api-spec.md                 # REST API spec (http://localhost:23001)
└── .updaterId                  # electron-updater id
```

Other user-level paths:
- `~/.alma/review-sessions/` — review-session artifacts
- `~/.cache/alma/` — non-config cache
- `~/Library/Application Support/alma/` — macOS-style dir (leftover / cross-platform code)
- `~/Documents/AlmaSync/` — user-visible sync folder
- `~/Downloads/alma-backup-2026-03-23*` — local backups

---

## 3. Skills System (the extension model)

Skills are the main way Alma is extended. Each skill is a folder with a `SKILL.md` (YAML frontmatter: `name`, `description`, `allowed-tools`; then a prose spec) and optional scripts.

**30 bundled skills** live in `/opt/Alma/resources/bundled-skills/`:

```
browser              memory-management    skill-hub            thread-management
discord              music-gen            skill-search         todo
file-manager         music-listener       system-info          travel
image-gen            notebook             tasks                twitter-media
plan-mode            reactions            telegram             video-reader
scheduler            screenshot           self-management      voice
selfie               send-file            self-reflection      web-fetch
web-search           xiaohongshu-cli
```

Some skills ship scripts alongside `SKILL.md` (e.g. `xiaohongshu-cli/scripts/xhs`).

**User-added / custom skills** are meant to live in `~/.config/alma/skills/` (currently empty on this machine) — and this repo (`/home/delta/workspace/ai/alma-skills/`) mirrors that model with `bundled/` + `custom/` directories, likely as a dev workspace for authoring/exporting skills.

---

## 4. Runtime Architecture (inferred)

1. `/opt/Alma/alma` boots Electron → loads `resources/app.asar` (main process).
2. Main process opens a Chromium window (renderer = the chat UI) using the user data dir `~/.config/alma/`.
3. A local HTTP server listens on **:23001** exposing a REST API (settings, providers, threads, skills, etc. — documented in `api-spec.md`).
4. Chat/thread state → SQLite (`chat_threads.db`) with `sqlite-vec` for semantic search and Drizzle for access.
5. Skills run in **sandboxed workspaces** (`workspaces/<id>/home/`) using bundled **bun / uv / rtk / node-pty**, so no reliance on the host's toolchain.
6. Local inference: onnxruntime + downloaded models in `embedding-models/` and `whisper_models/`.
7. Providers (Anthropic, etc.) called via their SDKs from the main process.
8. Optional Chrome extension (`/opt/Alma/resources/chrome-extension/`) talks to the app, likely over :23001.
9. Crashes/metrics → `sentry/` queue and Chromium `Crashpad/`.

---

## 5. Live State Snapshot (read from this machine)

- `tasks.json` currently has one in-progress task:
  `实现 auth/session/review 全链路（可 mock 验证）` (id `tmntu2ldqj7b0`, step 4/5, thread `mntu15vuohcjsyk78ef`, updated 2026-04-12).
- `workspaces/` has `default/` plus ~20 `temp-<id>/` scratch workspaces (Apr 7 → Apr 20).
- `groups/` shows integrations with **Discord** and **Feishu**.
- `people/` has one contact: `deltamx`.
- `plugins/` and `skills/` user dirs are empty — only bundled skills are active.
- `Preferences` is minimal: `{"migrated_user_scripts_toggle":true,"spellcheck":{"dictionaries":["en-US"]}}`.

---

## TL;DR

Alma = **Electron app** + **bundled Chromium** + **local SQLite/vector store** + **bundled bun/uv/rust/python runtimes** + **30 built-in "skills"** that run in **per-workspace sandboxes**, exposed through a **local REST API on :23001** and integrated with Discord / Feishu / browser extension. Installed system-wide under `/opt/Alma`; all your data lives under `~/.config/alma/`.
