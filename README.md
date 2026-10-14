<h1 align="center">Isaac Horowitz</h1>

<p align="center">
  <strong>I build AI systems, agent infrastructure, and native apps that do real work.</strong><br>
  Founder of <a href="https://ziplyne.agency">ZipLyne</a> · Miami, FL
</p>

<p align="center">
  <a href="https://www.iowitz.com"><img alt="iowitz.com" src="https://img.shields.io/badge/iowitz.com-111111?style=for-the-badge&logo=googlechrome&logoColor=white"></a>
  <a href="https://ziplyne.agency"><img alt="ZipLyne" src="https://img.shields.io/badge/ZipLyne-FF2E63?style=for-the-badge"></a>
  <a href="https://github.com/ZipLyne-Agency"><img alt="ZipLyne on GitHub" src="https://img.shields.io/badge/@ZipLyne--Agency-24292F?style=for-the-badge&logo=github&logoColor=white"></a>
  <a href="https://agent.ziplyne.agency"><img alt="My agent setup" src="https://img.shields.io/badge/My_agent_setup-2563EB?style=for-the-badge&logo=anthropic&logoColor=white"></a>
</p>

---

Most of what I ship sits where AI models meet the real world: a real browser with real logins, a real iPhone, a real Mac, real money, real customers. The models are the easy part. The hard part is making them safe and observable, and making them provably right when they act on something that matters. That is where I spend my time.

I work as a one person studio with a fleet of coding agents. Claude Code and Codex run side by side, hand work to each other, and review each other's changes. A lot of my open source work started as tooling I needed to run that fleet well.

## How I build

**Multi agent engineering.** My agents follow one shared rulebook and a routing graph that decides which model does what: a fast model for mechanical work, a stronger one for design, a different vendor for review so the reviewer does not share the author's blind spots. Commits, deploys, and migrations stay behind explicit human gates. The whole setup is documented at [agent.ziplyne.agency](https://agent.ziplyne.agency).

**Verification over vibes.** When an agent acts, the system should know whether it worked. TapLyne returns `verified`, `unverified`, or `failed` for every action and never retries an uncertain send or purchase. Grove's GitHub writes use previews that expire after five minutes and can only be used once. HebrewCore checks every translated sentence for prices, doses, and dates that changed.

**Local first AI.** When the data is personal, the model runs on the machine. Speech recognition runs on the Apple Neural Engine, vision and decision models run through MLX, and text cleanup uses Apple's on device model. Audio and video frames stay in memory and never leave the Mac.

**MCP as a first class surface.** Every product I build with an API also speaks Model Context Protocol, so agents can use it directly. Most also ship OpenAPI and `llms.txt`, and some are listed in the official MCP Registry.

**Production discipline on small tools.** Signed and notarized macOS builds, Sparkle auto updates checked against a pinned key, Keychain storage, no telemetry, secrets brokered from Infisical, and spend limits on the paths that can create a bill.

## Agent infrastructure

### [CodeGrid](https://github.com/ZipLyne-Agency/CodeGrid) · [codegrid.app](https://codegrid.app)
A workspace for running dozens of AI coding agents (Claude Code, Codex, Gemini, and more) on one infinite 2D canvas. Its **Agent Bus** is a built in MCP server over a local Unix socket that lets one agent message another, read its pane, and keep going, so Claude can hand a task to Codex and act on the reply. It tracks whether each agent is running, waiting, idle, or erroring, sends native notifications when one needs you, and broadcasts a prompt to every pane at once. Built with Tauri v2 and Rust, about 12 MB.

### [Agent Lanes](https://github.com/ZipLyne-Agency/Agent-Lanes)
Lets up to 64 agents use your real, logged in Chrome profile at the same time without ever touching your screen. Agents get private tabs inside four "lane" windows that live on an **invisible virtual display** kept alive by a launchd helper. Each lane runs a stage that shows one session's tab only while that session has a command running, because Chrome only renders active tabs. Input stays trusted browser input at about 61 fps while another app is in front. Agents reach the extension through a native messaging host and an owner only socket, so no pairing token ever lands in an MCP config, and the launcher refuses to connect unless Chrome's flags and the extension's hashes check out.

### [TapLyne](https://github.com/isaachorowitz/TapLyne-App)
Gives an AI agent a real iPhone with nothing installed on the phone. USB carries the screen through the capture device, and the Mac pretends to be a **Bluetooth mouse and keyboard** that AssistiveTouch turns into taps. Before each click it finds the on screen pointer and corrects its position, waiting for icon animations to settle. Every action needs a fresh `frame_id`, returns a new screenshot, and reports whether the input was delivered and whether the expected result appeared. Exposed as both MCP and REST, with an optional remote mode through a signed XCTest runner and an encrypted relay.

### [ZipLyne Lens](https://github.com/ZipLyne-Agency/Ziplyne-Lens)
Shows what AI coding actually costs, per project, repo, and model, by parsing the session logs Claude Code, Codex, and other CLIs already write. It also offers a live view of every agent session on the Mac and a searchable library of every prompt, with secrets stripped before anything is cached. Everything stays local.

## On device AI

### [Facet](https://github.com/isaachorowitz/Facet)
A private mirror for your workday. Cloudflare's **Clef-flash** decision model runs locally through MLX and reads expression, stress, fatigue, and focus about once a second. MediaPipe tracks 52 facial muscle activations, posture, and hand gestures at camera rate, and local speech models measure pace, pitch, and pauses. A local vision model narrates what you are doing. Everything is scored against your own calibrated baseline, and nothing but numbers is ever written to disk.

### [Keet](https://github.com/ZipLyne-Agency/keet)
Push to talk dictation that runs NVIDIA's **Parakeet** on the Apple Neural Engine: 30 to 70 ms to transcribe a few seconds of speech, with capture starting within about 30 ms of the key press. It keeps listening briefly after you release the key only if you are mid word, so the last word never gets cut. Filler words are removed by Apple's on device model under rules strict enough that it cannot change meaning. Hebrew gets its own key, running ivrit.ai's Whisper Turbo on the GPU through whisper.cpp.

## APIs and products for the Hebrew web

### [baba HebrewCore](https://github.com/ZipLyne-Agency/Hebrew-Core-by-baba) · [hc.itsbaba.com](https://hc.itsbaba.com)
The Hebrew API behind the [baba](https://www.itsbaba.com) translator app. Its **protected translation** between Hebrew, English, and Russian keeps prices, doses, codes, dates, and names exact and verifies every sentence. It has clinical, legal, and finance modes, gender aware output for speaker and listener, ICU plurals including the Hebrew dual, bidirectional text rendering for mixed Hebrew and English lines, code switching detection, transliteration, and nikud for text to speech. It ships as REST, OpenAPI, and a 16 tool remote MCP server.

### [baba News](https://github.com/ZipLyne-Agency/baba-Hebrew-Israel-News-API) · [news.itsbaba.com](https://news.itsbaba.com)
Israeli news in English and Hebrew with translated reporting and comparisons across newsrooms. It has 18 REST endpoints and 16 MCP tools, is listed in the official MCP Registry, and has plan quotas and rate limits.

### [Shelter Now](https://github.com/isaachorowitz/Shelters) · [getshelter.app](https://getshelter.app)
Finds the nearest bomb shelter in Israel in one tap, for the seconds after a siren. The dataset merges and de-duplicates **7,577 shelters across 56 cities** from more than 12 sources: municipal open data, the national CKAN catalog, OpenStreetMap, and Google Places. It is bundled into an offline PWA so it works with no network and no backend, falling back from PostGIS to static data automatically. Hebrew and English, free, MIT licensed.

## More things I've built

- **[Asteroid Map](https://github.com/ZipLyne-Agency/Asteroid-Map)** · [asteroidmap.com](https://asteroidmap.com): drops real NASA/JPL near Earth objects on any city and models crater, fireball, blast, and thermal zones from the Collins et al. (2005) impact equations, with WorldPop population exposure estimates.
- **[Grove](https://github.com/ZipLyne-Agency/Grove)**: a native GitHub repository manager that scans each repo's pinned Git tree to detect the services it uses, with optional on device Apple Intelligence and encrypted local storage.
- **[WhatsApp Tab Isolator](https://github.com/ZipLyne-Agency/WhatsApp-Tab-Isolator)**: runs several WhatsApp Web accounts in one Chrome profile by namespacing localStorage, IndexedDB, Cache Storage, and BroadcastChannel before page code runs.
- **[Witz Lyte](https://github.com/ZipLyne-Agency/WitzLyte)**: unlocks the full XDR brightness of compatible Mac displays through Apple's EDR pipeline, and dims below the system floor.
- **[SpaceNamer](https://github.com/ZipLyne-Agency/SpaceNamer)**: names macOS Spaces in Mission Control, tied to each Space's UUID, with layout save and restore.
- **[Mentionwell starters](https://github.com/ZipLyne-Agency/mentionwell-nextjs-starter)**: blog starters for [Mentionwell](https://mentionwell.com), which researches and publishes articles built for search and AI answer engines, with HMAC signed revalidation webhooks and `llms.txt`.
- **[Spotify Playlist Agent](https://github.com/ZipLyne-Agency/Spotify-Playlist-Agent)**: a guarded, bring your own app playlist migration toolkit designed around Spotify's AI content policy.

**In private and client work** I've built a server authoritative 1v1 real money arcade that settles USDC through an on chain escrow on Base, a Rust and Axum landed cost audit platform that publishes immutable, fully sourced reports, AI video production pipelines, and native Mac utilities that escalate meeting reminders to your phone and clean up the processes agents leave behind.

## Stack

<p>
  <img src="https://skillicons.dev/icons?i=ts,swift,rust,py,react,nextjs,tailwind,vite,tauri,nodejs,bun,cloudflare,postgres,supabase,docker,githubactions&perline=8" alt="TypeScript, Swift, Rust, Python, React, Next.js, Tailwind, Vite, Tauri, Node.js, Bun, Cloudflare, Postgres, Supabase, Docker, GitHub Actions">
</p>

**AI:** Claude and Codex as daily drivers, MCP servers and clients, MLX, Core ML and the Apple Neural Engine, Apple Foundation Models, MediaPipe, whisper.cpp, Parakeet, Playwright.<br>
**Apple:** SwiftUI and AppKit, EDR rendering, Bluetooth HID input, Keychain, Sparkle, Developer ID signing and notarization.<br>
**Web and infra:** Next.js, Cloudflare Workers, PostgreSQL and PostGIS, MapLibre, Infisical, Foundry and Base for on chain settlement.

## Get in touch

Building something that needs AI to do real work reliably? Reach me through [iowitz.com](https://www.iowitz.com) or [ziplyne.agency](https://ziplyne.agency).
