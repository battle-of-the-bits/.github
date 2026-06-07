# Battle of the Bits

**Configure your robot. Watch the AI fight. Export the clip.**

A free browser game — no download, no account needed. Build a robot with attribute sliders, pick a fight style, and watch two autonomous robots battle it out in a 3D arena. Record the fight and share it.

**[▶ Play now at botb.run](https://botb.run)**

---

![Battle preview](https://botb.run/og-image.png)

---

### Three robot types

| 🔨 Hammerbot | ⚙ Spinner | ↑ Flipper |
|:---:|:---:|:---:|
| High-power strikes | Gyroscopic blade | Hydraulic launch |
| Tune armor & reach | Tune RPM & mass | Tune flip force & wedge |

### Modes

- **AI vs AI** — configure both bots and watch the battle. Export the clip.
- **Human vs AI** — you control Robot A with WASD / arrow keys.
- **VS Friend** — share a room link, each player builds their robot, battle live.

### Robot customization

Pick from **8 colors**, tune 4 attribute sliders within a shared build budget, choose a fight style (Aggressive / Balanced / Counter), and **save up to 5 robots** to your profile. Saved robots can be reused across battles without reconfiguring.

### Available in EN / PT / ES

---

### Features planned

- 🏟️ Public arenas — publish your robot, share the link, let anyone challenge it
- 🏆 Global leaderboard — ranked wins, persistent robot profiles
- 🔓 Robot unlocks — new types unlocked by ranked wins
- 💻 Programmable robots — write JS to define your bot's behavior
- ⚔️ Ranked online multiplayer (long term)

---

### Architecture

No framework, no bundler. The entire frontend is vanilla JS + [Three.js](https://threejs.org) via CDN importmap — a git push to `main` is a full deploy, no build step.

The backend is TypeScript running on **Cloudflare Workers** at the edge, with a **Cloudflare D1** (SQLite) relational database and **Durable Objects** for stateful WebSocket rooms.

```
botb.run  (Cloudflare Pages + Workers)
    │
    ├── Auth          Google OAuth 2.0 → signed JWT in HttpOnly cookie
    ├── Database      Cloudflare D1 (SQLite) — users, robots, battle history
    ├── Real-time     Durable Objects — one WS room per battle, stateful
    └── Clip export   canvas.captureStream() → MediaRecorder → MP4/WebM
```

Migrations are SQL files applied per environment with Wrangler. Two environments: `staging` (preview Workers) and `production` (botb.run).

#### Implementation highlights

**Composite canvas recording** — the exported clip merges two canvas contexts: the WebGL frame from Three.js and a 2D canvas HUD (bot names, HP bars, winner banner) composited via `drawImage` on every `requestAnimationFrame` tick. `preserveDrawingBuffer: true` is required on the WebGL renderer — without it, `captureStream()` captures black frames.

**Deterministic cross-platform simulation** — battle outcomes are fully reproducible. Sin/cos arguments are normalized to `[0, 2π]` to eliminate floating-point divergence across Chrome, Safari, Firefox, and different operating systems. The simulation runs on the main thread at 60 fps with no replay system — what you watch is the live result.

**Stat-driven bot AI with physics** — each robot runs an AI loop driven entirely by its attribute sliders. Fight style affects energy drain and movement speed (`aggressive`: 1.5× drain / 1.1× speed; `counter`: waits for the opponent, breaks after 3.5 s stalemate). Approach angle is computed from the opponent's facing vector. Flipper attacks launch the opponent airborne with gravity (20 units/s²), tumble rotation, and a 0.65 s land stun.

**Durable Objects for VS Friend rooms** — each live battle room is a single Durable Object holding both WebSocket connections. No dedicated server, no shared state. The room is created on demand and destroyed when both players disconnect.

Also: **7-viewport Playwright test suite** (iPhone SE → Full HD) with guardrails that enforce canvas coverage, zero JS errors, HUD bounds, and virtual controls behavior on every commit. **PWA** — service worker cache-first strategy, installable on Android and iOS, fully offline after first load. **i18n** — EN / PT / ES with browser language auto-detection.

---

This project is **vibe coded** — built on weekends, shipped when it feels right.

---

📸 [@botb.run](https://instagram.com/botb.run) on Instagram  
🐛 [Found a bug or have a feature idea?](https://github.com/battle-of-the-bits/.github/issues/new/choose)
