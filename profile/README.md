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

No framework, no bundler. The entire frontend is vanilla JS + [Three.js](https://threejs.org) via CDN importmap. The backend is TypeScript, deployed globally.

```
botb.run  (frontend + API)
    │
    ├── Auth          OAuth 2.0 → signed JWT in HttpOnly cookie
    ├── Database      relational database — users, robots, battle history
    ├── Real-time     WebSocket rooms for live VS Friend battles
    └── Clip export   in-browser recording → MP4/WebM download
```

---

This project is **vibe coded** — built on weekends, shipped when it feels right.

---

📸 [@botb.run](https://instagram.com/botb.run) on Instagram  
🐛 [Found a bug or have a feature idea?](https://github.com/battle-of-the-bits/.github/issues/new/choose)
