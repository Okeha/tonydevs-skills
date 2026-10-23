---
name: saas-motion-design
description: Make a motion-design video for a SaaS product that shows the real, working UI in motion. Use when the user asks for a product video, launch film, demo reel, promo or explainer for a software product. Covers planning and the shot list, the script, theming from the product's own UI, motion and easing rules, UI choreography, camera, transitions, sync, optional voice-over, a pre-render quality check, and a stills review with the user before rendering.
---

# SaaS Motion Design

Make product films where the real product UI is the hero and every movement explains something.

## Defaults at a glance

| Setting | Default | Change only when |
|---|---|---|
| Aspect ratio | **16:9** (1920×1080) | The user explicitly asks for 9:16 |
| Frame rate | **60 fps** | The user asks for something else |
| Background | **Off-white** | The user specifies another background |
| Colors | Taken from the product's own UI theme | Never hard-code colors |
| Gradients | **None** | The user explicitly asks for them |
| Voice-over | **None** | The user explicitly asks for it |
| Music | A **high-energy** [Mixkit](https://mixkit.co/free-stock-music/) track the user picks | The user wants a calmer mood, nothing fits, or the film will be broadcast (see **Audio**) |

**One video per run.** Render a single aspect ratio. Never generate 16:9 and 9:16 together; a 9:16 cut is only made when the user asks for it.

## Tech stack

| Tool | Version | Used for |
|---|---|---|
| [Remotion](https://www.remotion.dev) | **4.0.535** | Building every scene as React components, previewing in Remotion Studio, exporting stills and rendering |
| [FFmpeg](https://ffmpeg.org) | Latest stable | Audio mixing, ducking the music under the voice, contact sheets, final encodes |
| [Node.js](https://nodejs.org) | **24** | Running Remotion and the build scripts |

- **Pin Remotion exactly.** Install every `remotion` and `@remotion/*` package at `4.0.535` with no `^` or `~`. Mismatched Remotion package versions break renders.
- **Check Node first.** Run `node -v` before installing anything. If it isn't v24, tell the user before going further.
- **Derive all motion from the frame.** Animate with `useCurrentFrame()` and `interpolate()`/`Easing.bezier()` using the easing presets below, never CSS transitions or timers, so every render comes out frame-exact.
- **Set the composition** to 1920×1080 at 60 fps by default, or 1080×1920 only when 9:16 is requested.
- **Stills** for the review step: `npx remotion still <composition> --frame=<n>`.
- **Final render:** `npx remotion render <composition> out/<name>.mp4 --codec=h264`.

## Workflow

1. **Plan.** Gather the brief and write the plan and shot list (see **Planning** below). Get the user's sign-off.
2. **Read the product.** Read the frontend codebase if you have it, otherwise the live UI, and extract its theme (see **Product access** and **Branding** below).
3. **Write the script** and get the user's sign-off (see **Script** below).
4. **Build the film** using the motion, choreography, camera and transition rules below, in the spirit of the **Inspiration** references.
5. **Add audio:** shortlist Mixkit tracks for the user to pick (see **Audio** below), then add SFX and, only if requested, the voice-over.
6. **Run the quality check.**
7. **Show stills and wait.** Show the user the review stills, answer their questions and make any changes (see **Stills review** below). Render only when the user approves.

Do not skip a sign-off step. Each one is cheaper to change than the step after it.

---

## Planning

Before writing any script, agree on what the film is for.

### 1. Gather the brief

Ask the user only for what you can't find out yourself:

| Question | Default if not given |
|---|---|
| What is the product, and can you share its frontend codebase? If not, where can the live UI be reached (URL, local build, login)? | Required. Ask. Prefer the codebase. |
| Who is the film for? | Prospective customers |
| What is the one thing the viewer should remember? | Required. Ask. |
| Which features or flows should it show? | The 2–3 flows that best prove the key message |
| How long should it be? | 30–45 s |
| Where will it be posted? Will it air on TV or radio? | Website and X/LinkedIn (16:9), online only |
| Should there be a voice-over, and if so, male or female? | No voice-over |
| What is the call to action? | Visit the product's URL |

### 2. Write the plan

Give the user a short plan to approve:

- **Goal:** one sentence on what the film must achieve
- **Audience:** who is watching, and what they already know
- **Key message:** the one line the viewer should remember
- **Format:** length, aspect ratio (16:9 unless 9:16 was asked for), voice-over yes or no
- **Story arc:** the structure template (see **Script**) filled in for this product, one line per section
- **Flows to capture:** the exact UI screens and interactions to record, with the data state each one needs (for example "project board with 12 tasks, 3 of them overdue")
- **Proof:** the evidence the film will show, if any (a metric, customer logos, a quote, a benchmark or an award), and where it comes from

### 3. Write the shot list

Break the story arc into shots, one row per shot:

| # | Section | Shot | Motion | Duration |
|---|---|---|---|---|
| 1 | Hook | Typed question in the product's search bar | Caret types, answer streams in | 4 s |

The shot list becomes the "What's on screen" column of the script.

---

## Product access

The film is only as good as your access to the product. In order of preference:

| Access | What it gives you |
|---|---|
| **1. Frontend codebase (best)** | Exact design tokens, real components, real copy, icons and logo files. The UI can be rebuilt inside Remotion pixel-for-pixel and animated freely. |
| **2. Running app** (URL or local build, with a test login) | The real theme and flows, captured from the browser. Faithful, but harder to animate element by element. |
| **3. Screen recordings or screenshots** | A last resort. Rebuild the screens from them as closely as you can, and tell the user the result will be less exact. |

If you don't have the codebase, ask for it before settling for a lower option.

### Working from the codebase

- **Theme:** read the design tokens from wherever the project keeps them, such as CSS custom properties, the Tailwind config, a theme file or a design-tokens package.
- **Components:** reuse the product's own UI components in the Remotion compositions where they render cleanly outside the app. Otherwise recreate them faithfully from their source.
- **Content:** take real labels, empty states, icons and logo files from the source instead of inventing them. Use seed or fixture data for the data states the plan needs.
- **Read only.** Never edit the product's codebase. Build the film in its own Remotion project.

---

## Branding

Do not use any fixed brand colors. Every color comes from the product being shown.

- **Read the core UI of the product**, from the codebase when you have it, and pull its theme: primary/accent color, neutrals, surface colors, border colors, typography, corner radii and shadow style.
- **Background:** favour an **off-white** background by default unless the user specifies otherwise.
- **Accent:** use the product's own accent color for emphasis, cursor rings, carets and highlights.
- **No gradients** in backgrounds, text, glows or transitions unless the user explicitly asks for them. Use flat fills.
- **Typography:** use the product's own typeface where possible, so captions feel native to the UI.
- **Logo and wordmark:** use the product's real assets. Never redraw or approximate them.

## Script

The script drives everything: captions, timing, and the voice-over if there is one. Write it from the approved plan and shot list, before building any scene, and get the user's sign-off before you build.

### Format

Write the script as a table, one row per line:

| # | Line (spoken or on-screen) | What's on screen | Duration |
|---|---|---|---|
| 1 | Your team loses six hours a week to status updates. | Counter climbs to 6 over a cluttered task list | 3 s |

### Rules

- **Hook in the first 7 seconds.** Open with the problem, a surprising number or the product doing something delightful, not the product name.
- **Every line has a picture.** If a line has no matching visual beat, cut it or rewrite it.
- **Plain language.** Short sentences, one idea per line, no jargon the viewer has to decode.
- **Pace:** about 2.5 words per second when voiced. Budget the line's duration from its word count.
- **Mark emphasis** with `*stars*`, one key phrase per line at most.
- **Every claim is true.** Numbers and benchmarks must be real and footnoted where needed.
- **Respect the length** set in the brief. Trim lines before speeding up motion.
- **No voice-over by default.** Without one, the script appears as on-screen captions only.

### Structure template

This is the default arc. Adapt it to the product and the brief.

1. Hook (0–7 s)
2. Problem
3. Brand reveal
4. Product, stage by stage, on the real UI
5. Proof (optional): a metric, customer logos, a quote, a benchmark (with footnote) or an award
6. Call to action, ending on the product's URL
7. Logo sting (2.8 s)

For a feature launch or a playful product, a **show, don't tell** arc often works better: hook → the product in action → one key line → logo. Skip the problem section if the problem is obvious.


---

## Inspiration

Watch these before building. They set the bar for feel, pacing and how much the UI carries the story:

- [Evernote: AI-Powered Search](https://x.com/evernote/status/1719024066072317988)
- [Figma: FigJam AI](https://x.com/figma/status/1721936196996350387)
- [Nexoxh: motion reel](https://x.com/nexoxh/status/2108496742383513615)
- [LexnLin: Tessel launch film](https://x.com/lexnlin/status/2104148233106723099)

Take these traits from them. They describe technique only. Colors always come from the product being shown (see **Branding**).

- **Calm opening.** Open on a near-empty off-white frame with one small element (a cursor, a logo mark, a dot), then build from there.
- **The product is simplified, not invented.** Show real UI, but trim it to the elements that matter: flat thumbnails, clean placeholder content, no clutter.
- **One UI element, huge.** Lift a single control (a toolbar icon, a search bar, an AI panel) out of the interface and show it big and centred before showing it in context.
- **The interaction tells the story.** A question is typed into the product, the answer streams in, and the key phrase in the answer is highlighted in the accent color.
- **Busy, then sorted.** Show the messy state first (scattered notes, many live cursors, overflowing lists), then let the product tidy it in one satisfying move.
- **Signs of life.** Multiple named cursors, reactions and live edits make the product feel used by real people.
- **Flat color fields as transitions.** A solid shape from the product palette floods the frame to change scene. Use flat fills only, never gradients.
- **Type with character.** Pair one bold face with one contrasting face (for example an italic serif) for headlines, and mark one word per headline with a flat highlighter bar.
- **Chips and numbers as props.** Floating task chips, countdown timers and counters that tick up give abstract claims a physical form. Motion blur on fast-moving numbers and type sells speed.
- **Quiet frame details.** Thin corner marks and small metadata labels (timecode, scene number) at the frame edges can add polish. Keep them faint and never let them compete with the UI.
- **One recurring motif.** A single small mark in the accent color (a dot, the cursor, part of the logo) opens the film, carries the transitions and closes it. It ties the film together.
- **Neutral UI, one accent.** Keep the UI mostly neutral so the accent color appears only where attention should go.
- **Headlines over blurred UI.** Put big, bold headlines over the product UI blurred behind them. The words land, then the UI comes into focus.
- **Split frame.** Put the headline on one side and a tight crop of the UI proving it on the other ("Meetings move over." next to the calendar rearranging).
- **Natural-language prompt.** A prompt bar types a plain request and the UI visibly rearranges to fulfil it.
- **Zoom out to a pattern.** Pull back until the UI's blocks become an abstract repeating pattern, then land the closing line over it.
- **Clean ending.** Resolve to the logo or wordmark plus one short line, then hold.

Do not copy these films' colors, characters or brand shapes.

---

## Video

Use **live flows of the actual UI**: real screens, real interactions, real data states. Do not rely on static screenshots; they feel lifeless.

### 1. Motion principles

- **Motion explains the product.** Every move points at something that matters. Nothing moves just to fill time.
- **One focal point per moment.** Everything else dims, blurs or steps back.
- **The product UI is the hero.** Type and effects support it and never compete with it.

### 2. Easing and timing

#### Easing presets

| Name | Curve | Use |
|---|---|---|
| `EO` (ease-out) | `cubic-bezier(0.16, 1, 0.3, 1)` | Entrances: fast in, soft landing |
| `EIO` (ease-in-out) | `cubic-bezier(0.65, 0, 0.35, 1)` | Camera moves, position changes |
| `EI` (ease-in) | `cubic-bezier(0.7, 0, 0.84, 0)` | Exits |
| `POP` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Pills, chips, badges, stamps (slight overshoot) |

#### Durations

| Motion | Duration |
|---|---|
| Entrance | 0.6–0.9 s |
| Exit | 0.3–0.35 s |
| Pop | 0.3–0.4 s |
| Camera push | 1.0–1.6 s |
| Stagger between items | 0.1–0.3 s |
| Caption stagger | 3–5 frames per word |
| Minimum hold on readable text | 1.5 s |

### 3. Text animation

- **Captions:** words rise from about 0.45 em below, un-blurring from 12 px to 0. On exit they blur out and drift up 18 px.
- **Emphasis:** the `*starred*` phrase takes the product's accent color.
- **Typed text:** a caret in the product's accent color, timed to the line. Used for hooks.
- **Counters:** numbers count up as the line names them.
- **Limits:**
  - No more than **two lines** of on-screen text at once.
  - In 9:16 (when requested), top-of-frame headlines move into the header band above the stage.

### 4. UI choreography

Use whichever patterns fit the product's UI. A canvas tool, an editor and a dashboard each move differently.

- **Window entrance:** perspective tilt (rotateX about 22°, settling to about 7°) while the window rises 300–400 px.
- **Lists and tables:** rows stream in one by one. Badges and status pills **pop after** the row lands, not with it.
- **Canvases and editors:** objects, text and cursors appear the way a real user would create them, not all at once.
- **Cursor:**
  - glides along an ease-in-out path
  - presses with a scale of 0.88
  - a ring in the product's accent color ripples out as the click lands on its word
- **Reveals:** data fills in **while the line describes it**, in the order it is named.
- **Card stack:** each new card comes forward while the previous one steps back: it moves left about 300 px, scales to 0.84, blurs 4 px and drops to 45% opacity.

### 5. Camera

- **Slow drift:** a 2–4% scale creep over a scene, so nothing is ever fully static.
- **Push-in to the hero element:** scale 1.4–1.75, with everything else dimmed or blurred.
- **Freeze and push:** a moving view (a feed, a board, a timeline) stops and the camera moves into one item.
- **Banned:** camera shake and spinning.

### 6. Transitions

Keep the vocabulary small and reuse it:

| Transition | When |
|---|---|
| Blur-out to blur-in | Default cut between scenes |
| Cut to empty frame | The single most important line. The music drops out and the text lands in near-silence. |
| Brand reveal | The logo mark appears, the wordmark draws in, the tagline settles |
| Tone shift | The flat background tint shifts from a muted problem tone to the product's accent tone, from problem to answer (no gradient) |
| Symbol still in | Opening: the logo mark fades in over 1.4 s |
| Logo sting out | Closing: the brand reveal, then the product URL, 2.8 s |

**Banned:** generic wipes, spins, glitch effects, particle bursts, and everything fading in at the same time.

### 7. Sync rules

- **Picture serves the script:** every visual beat lands on the word that names it. For example, the counter hits 6 on "six hours".
- **Cut on the music:** scene cuts fall on the beat or a phrase change, and the logo lands on a music break.
- **Time-warping existing footage:**
  - keep playback speed between **0.15× and 1.7×**
  - use smooth curves (PCHIP) rather than sudden speed changes

### 8. Aspect ratio

- **16:9 is the default** and the only format rendered unless the user asks otherwise.
- **9:16 only on explicit request.** When requested, render 9:16 *instead of* 16:9, using the `Portrait` component (scaled stage plus header band). Never render both in the same run.

---

## Audio

- **Music:** one bed that sets the pace; scene cuts follow its beats and phrase changes.
- **SFX follow motion:** mark motion events only (stamp, click, pop, whoosh, chime). Keep them sparse and quiet. Synthesize them in code.
- **With a voice-over:** duck the music and SFX further under the voice.

### Music source: Mixkit

Use a real track from [Mixkit](https://mixkit.co/free-stock-music/) instead of synthesized music. Mixkit's free licence allows online ads, websites, social media and YouTube, with no credit required.

**Check the licence fits first.** Mixkit music may **not** be used in TV or radio broadcasts, CDs, DVDs or video games. If the brief mentions any of these, stop and tell the user they need a paid licence from another library. Read the current terms at [mixkit.co/license](https://mixkit.co/license/) once per project in case they have changed.

**Find and choose a track:**

1. **Favour high energy by default.** Unless the user or the plan asks for something calmer, look for driving, upbeat tracks with a strong beat that push the film forward. Start from Mixkit's mood pages at `https://mixkit.co/free-stock-music/mood/<mood>/`, using `energetic`, `driving`, `upbeat`, `propulsive`, `dynamic`, `exciting`, `confident` or `euphoric`. Narrow down with tag pages at `https://mixkit.co/free-stock-music/tag/<tag>/`, such as `technology`, `corporate`, `motivational`, `intro`, `trailer` or `sports`. Use calmer moods (`calm`, `atmospheric`, `dreamy`) only when the brief asks for them.
2. **Shortlist 3–5 tracks** from those pages. Each card shows the title, artist and duration, and the preview MP3 is at `https://assets.mixkit.co/music/<id>/<id>.mp3`. Skip tracks shorter than the film unless they loop cleanly, and skip vocals unless the user wants them.
3. **Download the candidates** into the video project's `audio/music/candidates/` folder.
4. **Analyse each one with FFmpeg:**
   - measure the loudness envelope (for example with `astats` per window) to find the build, the drop and any quiet passages
   - estimate the tempo, so cuts can sit on the beat grid; for the high-energy default, prefer roughly 115 BPM or faster, with a beat that comes in early and holds steady
   - drop tracks with a long quiet intro or that stay low-energy for most of their length, unless the brief wants calm
   - find a phrase change for the brand reveal, a break for the logo, and a quiet spot for the "cut to empty frame" line
5. **Present the shortlist** to the user: title, artist, Mixkit page link, duration, tempo, and one line on why it fits the plan. The user picks by ear; don't choose for them.
6. **Record the licence.** Write `audio/music/LICENSE.md` in the video project with the track title, artist, Mixkit page URL, licence URL and download date. Delete the candidates that weren't chosen.

**Edit to the track:**

- Fit the film to the music, not the other way round. Trim, fade or loop at phrase boundaries; never time-stretch or pitch-shift the track.
- Use the track's own sections: the hook over the intro, the product over the main groove, the logo on the final hit or break.
- End with a clean fade or the track's own ending, never a hard cut mid-phrase.

**Rules:**

- **Never commit Mixkit audio** to a public repository or share the raw files. They may only be used inside the finished video.
- **Never use a track** without a recorded `LICENSE.md` entry.
- **Fallback:** if nothing on Mixkit fits, or the user declines, synthesize a simple music bed in code and tell the user.

## Voice

> Voice-overs are **strictly optional** and only used when the user requests one.

- Use **Eleven v4** on OpenRouter, model ID `elevenlabs/eleven-v4`.
- **Voices:** Brian when a male voice is requested, Matilda when a female voice is requested. Default to **Brian** if no gender is specified.
- **Cost:** before generating, research the current cost to generate the voice-over for this script's length and tell the user the estimate.

---

## Quality check before rendering

- [ ] Contact sheet with one frame per script line; the picture matches the words
- [ ] Only one aspect ratio is being rendered (16:9 unless 9:16 was requested)
- [ ] If 9:16: nothing is cut off
- [ ] All colors come from the product's UI; background is off-white unless specified
- [ ] No gradients, unless requested
- [ ] Nothing moves without a reason
- [ ] Every number on screen is true and footnoted where needed
- [ ] Length is within the limit set in the brief
- [ ] The music track has a `LICENSE.md` entry, and the film isn't going to TV, radio, CD, DVD or a game
- [ ] Music ends on a phrase or fade, not a hard cut

## Stills review

Before the final render, show the user stills so they can ask questions and request changes while changes are still cheap.

1. **Export stills:** one frame per script line, taken at the moment the line's key visual lands, plus the first and last frames.
2. **Present them as a contact sheet**, each still labelled with its shot number, script line and timestamp.
3. **Invite questions.** Ask the user whether anything is unclear, wrong or off-brand, and whether they want any changes.
4. **Revise and re-show** the changed stills until the user is happy.
5. **Render only after the user explicitly approves.** If they have no questions, a simple "looks good" is enough.

## Reusable components

Keep these as Remotion components in the skill's `templates/` folder so every film shares the same motion:

| Component | Purpose |
|---|---|
| `Caption` | Word-by-word headline with accent emphasis and the 9:16 header slot |
| `LogoReveal` | The brand reveal |
| `Sting` | The closing logo |
| `SymbolIntro` | The opening still |
| `Cursor` | Glide, press and ripple |
| `Stacked` | The card-stack entrance and step-back |
| `Portrait` | The 9:16 frame with the scaled stage and header band (only when 9:16 is requested) |
| Easing presets | `EO`, `EIO`, `EI`, `POP` |

All components read their colors from the product theme; none of them hard-code a color.
