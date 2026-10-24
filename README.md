# tonydevs-skills

Claude skills by tonydevs.

| Skill | What it does |
|---|---|
| [`saas-motion-design`](saas-motion-design/skills/saas-motion-design/SKILL.md) | Makes motion-design product videos for SaaS. It uses the product's real, working UI and takes its colors from the product's own theme. |

## saas-motion-design

Ask Claude for a product video, launch film, demo reel or explainer for your software product. The skill then:

1. **Plans the film** with you: the brief, a one-page plan and a shot list, for you to approve.
2. **Reads your product's UI** and takes its colors, type and styling from it. It never hard-codes colors, uses an off-white background by default and avoids gradients unless you ask for them.
3. **Writes a script** for you to approve before anything is built.
4. **Builds the film** from live UI flows, not static screenshots, following fixed rules for easing, timing, camera moves, transitions and sync. Its style is based on launch films from Evernote, Figma and Tessel (by LexnLin) and a motion reel by Nexoxh.
5. **Shortlists music from [Mixkit](https://mixkit.co/free-stock-music/)**, which is free for online ads and social with no credit needed. You pick the track by ear, and Claude cuts the film to its beats and records the licence details.
6. **Shows you stills** of every key moment, answers your questions and makes your changes before rendering.
7. **Renders one video**: 16:9 by default, or 9:16 only when you ask for it. It never renders both formats in the same run.

You only get a voice-over if you ask for one. It is generated with Eleven v4 (`elevenlabs/eleven-v4`) on OpenRouter, and Claude tells you the estimated cost before generating it.

### Best results: give Claude your frontend codebase

The skill works best when Claude can read your product's frontend code. It can then take the exact colors, fonts and components from the source and rebuild your UI faithfully in the video. Run Claude Code from your frontend repo, or point it at the repo's path.

If you can't share the code, a running app (a URL or local build plus a test login) works too. Screenshots or screen recordings are a last resort, and the result will be less exact.

### Example prompts

```
Make a 45-second launch video for our dashboard at https://app.example.com
```

```
Make a 30-second 9:16 product video for Instagram, with a female voice-over
```

## Installation

### Easiest: ask Claude to install it

Copy this repository's link and paste it into Claude Code with a request like:

```
Install and use the saas-motion-design skill from https://github.com/okeha/tonydevs-skills
```

Claude will add the marketplace and install the plugin for you, or copy the skill into `~/.claude/skills/`. Then you can go straight to asking for a video. If it asks for permission to run commands, approve them.

### Claude Code: plugin marketplace (recommended)

In Claude Code, run:

```
/plugin marketplace add <your-github-username>/tonydevs-skills
/plugin install saas-motion-design@tonydevs-skills
```

Restart Claude Code if it asks you to. To get later versions, run `/plugin marketplace update tonydevs-skills`.

### Claude Code: manual install

Copy the skill folder into your personal skills directory, which makes it available in every project:

```bash
git clone https://github.com/<your-github-username>/tonydevs-skills.git
mkdir -p ~/.claude/skills
cp -r tonydevs-skills/saas-motion-design/skills/saas-motion-design ~/.claude/skills/
```

To install it for a single project instead, copy the folder to `.claude/skills/` inside that project.

### Claude.ai and Claude Desktop

1. Zip the skill folder:
   ```bash
   cd tonydevs-skills/saas-motion-design/skills
   zip -r saas-motion-design.zip saas-motion-design
   ```
2. In Claude, go to **Settings → Capabilities → Skills** and upload `saas-motion-design.zip`.
3. Make sure **Code execution** is turned on, because skills need it.

### Check that it works

Ask Claude *"What skills do you have available?"*. `saas-motion-design` should be in the list. In Claude Code, the skill also triggers on its own when you ask for a product video.

## Requirements

| Tool | Version | Install |
|---|---|---|
| Node.js | 24 | [nodejs.org](https://nodejs.org) or `nvm install 24` |
| Remotion | 4.0.535 | Installed per project by the skill (`npm i remotion@4.0.535 @remotion/cli@4.0.535`) |
| FFmpeg | Latest stable | `brew install ffmpeg` (macOS), `sudo apt install ffmpeg` (Debian/Ubuntu), `winget install ffmpeg` (Windows) |

- **For the voice-over (optional):** an [OpenRouter](https://openrouter.ai) API key with access to `elevenlabs/eleven-v4`.
- **Music licence:** Mixkit's free licence covers web, social and online ads. It does **not** cover TV or radio broadcast, CDs, DVDs or video games; use a paid music licence for those. Never commit downloaded tracks to a public repo. See [mixkit.co/license](https://mixkit.co/license/).
- **Remotion licence:** Remotion is free for individuals and small teams, but larger companies need a paid company licence. Check [remotion.dev/license](https://www.remotion.dev/license) before using it commercially.

## Repository layout

```
tonydevs-skills/
├── .claude-plugin/
│   └── marketplace.json          # marketplace listing
└── saas-motion-design/           # the plugin
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        └── saas-motion-design/
            └── SKILL.md          # the skill itself
```

## Contributing

Issues and pull requests are welcome. If you change motion values, update both the tables in `SKILL.md` and any matching components in `templates/`.
