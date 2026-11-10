---
theme: ./theme
title: Running Storybook inside your Vite dev server
info: |
  ## JSPoland 2026
  Running Storybook inside your Vite dev server — and what Storybook 11 does with it.
class: text-center
drawings:
  persist: false
transition: slide-left
comark: true
duration: 18min
---

# Running Storybook inside your <SBGradientText>Vite dev server</SBGradientText>

<div class="mt-8 opacity-70">Julien Huang · JSPoland 2026</div>

<!--
[0:00 · 10s] Title.
-->

---

# Hi, I'm Julien

- Open-source enthusiast since 2021
- <logos-chromatic-icon /> Open-source engineer at Chromatic
- <logos-storybook-icon /> Storybook core team
- <logos-nuxt-icon /> Nuxt core team
- <carbon-logo-github class="text-white" /> @huang-julien
- <logos-bluesky /> @julienhuang-dev.bsky.social
- <carbon-logo-x class="text-white" /> @JulienHuang_dev


<!--
[0:10 · 50s]
-->

---
layout: intro
---

# Working on a feature as a frontend dev

<!--
[1:00 · 15s] Set the scene: you're building a component, you want to see it in the app AND in isolation.
-->

---

# Starting 2 terminals

<div class="grid grid-cols-2 gap-6 mt-8">

<SBWindow title="Terminal 1">

```bash
$ npm run dev

  VITE v7  ready in 312 ms
  ➜  Local: http://localhost:5173/
```

</SBWindow>

<SBWindow title="Terminal 2">

```bash
$ npm run storybook

  Storybook 11 started
  ➜  Local: http://localhost:6006/
```

</SBWindow>

</div>

<div class="mt-10" />

- Two terminals to start (and to remember to start)
- Two ports, two browser tabs

<!--
[1:15 · 45s] Two commands, two processes, two ports. Everyone in the room has done this.
-->

---
layout: statement
---

# What if Storybook ran in the dev server <SBGradientText>you already started?</SBGradientText>

<!--
[2:45 · 15s] Transition.
-->

---

# Storybook Labs

<div class="mt-6 text-xl">

Experiments from the Storybook team

</div>

<v-clicks>

- Public, in the open: `storybookjs/labs`
- May change, may disappear
- Your feedback decides what graduates

</v-clicks>

<div v-click class="mt-10">
<SBCallout title="Experiment #1">
Storybook as a Vite plugin
</SBCallout>
</div>

<!--
[3:00 · 1:00] Frame it: this is where we try things. "Here's the first one."
-->

---

# Storybook as a Vite plugin

```ts {all|2|6-8}
import { defineConfig } from 'vite'
import { experimental_vitePlugin } from '@storybook/vite-plugin'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [
    react(),
    experimental_vitePlugin(),
  ],
})
```

<!--
[4:00 · 1:00] That's the whole setup. Your existing .storybook folder stays as-is.
-->

---

# One dev server

<div class="grid grid-cols-2 gap-8 mt-8 text-center">
<div>
<div class="text-5xl font-black">/</div>
<div class="opacity-70 mt-2">your app</div>
</div>
<div>
<div class="text-5xl font-black"><SBGradientText>/__storybook</SBGradientText></div>
<div class="opacity-70 mt-2">Storybook</div>
</div>
</div>

<div class="mt-12">

| Command | Result |
| --- | --- |
| `vite` | App + Storybook, same port |
| `vite --mode storybook` | Storybook only, at `/` |
| `vite build --mode storybook` | Static Storybook |
| `vite build` | Your app. **Untouched.** |

</div>

<!--
[5:00 · 45s] Say it out loud: vite build is unaffected, nothing from Storybook ships in your app — the plugin doesn't even activate.
-->

---
layout: intro
---

# Demo

<div class="opacity-70">mealdrop · one terminal</div>

<!--
[5:45 · 2:30] LIVE DEMO
1. `pnpm dev` — one terminal
2. Open localhost:5173 → the app
3. Open localhost:5173/__storybook → Storybook
4. Edit a component → HMR in both
5. LEAVE IT RUNNING — we come back to it for SB 11
Backup: recorded video in case of trouble.
-->

---

# How it works

<div class="flex flex-col items-center gap-4 mt-6 text-sm">

<div class="px-6 py-3 rounded-lg border-2 border-white/30 font-bold">HTTP server · localhost:5173</div>

<div class="text-2xl opacity-50">↓</div>

<div class="grid grid-cols-2 gap-6 w-full">
<div class="p-4 rounded-lg border border-white/20">
<div class="font-bold mb-2">Your Vite dev server</div>
<div class="opacity-70"><code>/</code> → your app<br/>your HMR</div>
</div>
<SBCallout title="Storybook middleware">
<div class="opacity-80"><code>/__storybook</code> → manager<br/>preview: its own Vite + HMR socket</div>
</SBCallout>
</div>

</div>

<!--
[8:15 · 45s] Storybook is mounted as middleware on your dev server. Same port, same lifecycle.
The preview still runs its own Vite instance (own HMR socket at /__storybook-hmr) — it's the same Storybook as before, just mounted.
-->

---

# Not just Vite apps

<div class="grid grid-cols-[0.95fr_1.25fr_1.05fr] gap-4 mt-8">

<div>
<div class="flex items-center gap-2 font-bold mb-2"><logos-vitejs /> Vite</div>

<SBWindow title="vite.config.ts">

```ts
plugins: [
  experimental_vitePlugin(),
]
```

</SBWindow>
</div>

<div>
<div class="flex items-center gap-2 font-bold mb-2"><logos-angular-icon /> Angular CLI</div>

<SBWindow title="angular.json">

```json
"serve": {
  "builder":
    "@storybook/vite-plugin/angular:dev-server"
}
```

</SBWindow>
</div>

<div>
<div class="flex items-center gap-2 font-bold mb-2"><logos-nextjs-icon /> Next.js</div>

<SBWindow title="server.mjs">

```js
import { startNextDevServer }
  from '@storybook/vite-plugin/nextjs'

await startNextDevServer()
```

</SBWindow>
</div>

</div>

<div class="mt-10 text-center text-2xl">

Same port, <SBGradientText>whatever your dev server</SBGradientText>

</div>

<style>
.slidev-code { font-size: 0.62rem !important; line-height: 1.5 !important; }
</style>

<!--
[9:00 · 30s]
- Vite: the plugin (React, Vue, Svelte… anything on Vite)
- Angular: `ng serve` doesn't load a Vite config → a dev-server builder instead. Needs @angular/build 21 and @storybook/angular-vite
- Next.js: no dev-server plugin API → a custom server replaces `next dev` (`"dev": "node server.mjs"`). Needs @storybook/nextjs-vite. Existing custom server? `createStorybookMiddleware`
- Nuxt works too in the labs examples (via a local module)
TODO: release the Angular / Next.js adapters (pending changesets after 0.1.0) before the talk.
-->

---

# Compatibility

<div class="flex gap-6 mt-10 text-2xl">
  <SBBadge>Storybook 11</SBBadge>
  <SBBadge>Vite ≥ 6.3</SBBadge>
  <SBBadge>Node ≥ 22.12</SBBadge>
</div>

<div v-click class="mt-16 text-2xl">

…and Storybook 11 is where it gets interesting.

</div>

<!--
[9:30 · 30s] Bridge to the second half.
-->

---
layout: full
---

<img src="./theme/assets/sb-11/sb_11.png" class="absolute inset-0 h-full w-full object-contain" />

<!--
[10:00 · 15s] Section opener.
-->

---

# The missing piece of Storybook

<logos-github-icon v-drag="[570,330,60,65]" />
<logos-storybook-icon  v-drag="[224,298,62,55]" />
<logos-chromatic-icon  v-drag="[742,337,76,69]" />
<ant-design-team-outlined v-drag="[635,440,68,57]"  />
<ant-design-user-outlined  v-drag="[146,384,74,58]" />
<ant-design-robot-filled v-drag="[299,383,54,60]" />

<!--
[10:15 · 30s] Ported from ZurichJS — reposition icons with v-drag if the theme differs.
Your agent needs the same thing you do: a place to see and test components.
(Trimmed to 30s to make room for the benchmark slide.)
-->

---


# Your agent connects here

<div class="mt-10 text-center text-4xl font-black">

`localhost:5173`<SBGradientText>/__storybook/mcp</SBGradientText>

</div>

<div class="mt-10 text-center opacity-70">The same dev server, still running from the demo</div>

<!--
[11:30 · 45s] MCP server (@storybook/addon-mcp): your agent gets component docs, story tests and review as tools.
MCP works in every framework; the docs tools (docs-list / docs-show) need `features.componentsManifest` and work with React, angular-vite and vue3-vite.
Docs default endpoint is localhost:6006/mcp (standalone).
The plugin now writes the instance registry (labs #11), including the MCP endpoint, so agents and `storybook tools` should discover it.
TODO before recording: verify /__storybook/mcp responds through the plugin.
-->

---

# Skills <span class="opacity-40">+</span> CLI

<div class="grid grid-cols-2 gap-8 mt-6">

<div>

### <SBGradientText>Skills</SBGradientText> · what to do

<SBWindow title="Terminal" class="mt-3">

```bash
npx storybook skills ui-workflow
npx storybook skills component-docs
npx storybook skills write-story
npx storybook skills setup
```

</SBWindow>

<div class="mt-4 text-sm opacity-80">

Get them from <SBGradientText>skills.sh/storybookjs/storybook</SBGradientText><br/>
or the Claude Code & Codex plugins<br/>
`/storybook:ui-workflow`

</div>
</div>

<div>

### <SBGradientText>Tools</SBGradientText> · how to do it

<SBWindow title="Terminal" class="mt-3">

```bash
npx storybook tools docs list
npx storybook tools docs show --id button
npx storybook tools stories changed
npx storybook tools test run
npx storybook tools review create
```

</SBWindow>

<div class="mt-4 text-sm opacity-80">

The same tools as MCP: `docs list` = `docs-list`<br/>
Attaches to your running Storybook

</div>
</div>

</div>

<!--
[12:15 · 1:00] Skills = instructions, tools = actions. No MCP setup needed: any agent with a shell can use them.
- ui-workflow: look up components → write stories → run tests → publish a review
- component-docs: props & usage, so the agent reuses components instead of rebuilding them
- write-story / setup: conventions for this project / agentic setup
- Distribution: skills.sh/storybookjs/storybook (any agent), or the Claude Code & Codex plugins (`claude plugin install storybook@storybook`)
- `AGENTS.md` tells the agent when to use them
- `storybook upgrade` and `create storybook` install the skills and add the AGENTS.md section (BEGIN/END:storybook-skills markers)
- `storybook ai` from SB 10 is gone → `storybook skills setup` / `storybook tools`
- CLI default mode: attaches to a running instance, otherwise loads the config locally (--attach / --no-attach)
TODO: confirm `npx storybook tools --attach docs list` attaches to the plugin-hosted Storybook (the plugin writes the instance registry since labs #11).
-->

---
layout: intro
---

#### `/storybook:ui-workflow Add dietary tags to the restaurant menu in mealdrop.`

<!--
[13:15 · 15s] The prompt.
-->

---

# Does it actually help?

<div class="opacity-70 mt-6">Our benchmark: 650 agent sessions on MealDrop, with vs. without Storybook MCP</div>

<div class="grid grid-cols-4 gap-5 mt-10 text-center">
<SBCallout>
<div class="text-5xl font-black"><SBGradientText>23%</SBGradientText></div>
<div class="mt-2 font-bold">faster</div>
<div class="text-xs opacity-70 mt-1">time to complete</div>
</SBCallout>
<SBCallout>
<div class="text-5xl font-black"><SBGradientText>~10%</SBGradientText></div>
<div class="mt-2 font-bold">cheaper</div>
<div class="text-xs opacity-70 mt-1">18% fewer input tokens</div>
</SBCallout>
<SBCallout>
<div class="text-5xl font-black"><SBGradientText>34%</SBGradientText></div>
<div class="mt-2 font-bold">less digging</div>
<div class="text-xs opacity-70 mt-1">codebase reads & searches</div>
</SBCallout>
<SBCallout>
<div class="text-5xl font-black"><SBGradientText>~15%</SBGradientText></div>
<div class="mt-2 font-bold">less code</div>
<div class="text-xs opacity-70 mt-1">for new UI (13–16%)</div>
</SBCallout>
</div>

<div v-click class="mt-10 text-center text-xl">

More content ≠ better: <b>focused docs</b> were ~⅓ faster and 17–23% cheaper.<br/>
<span class="opacity-70">The fullest Storybook did no better than no Storybook.</span>

</div>

<SourceNote>Storybook agentic reference benchmark · MealDrop + Droppy design system · Claude Opus 5 (high) · 5 tasks × 10 runs × 13 configurations</SourceNote>

<!--
[10:45 · 45s] Same app as the demo! Say "in our benchmark", never "Storybook makes agents 23% faster".
- 23% faster: UI rework −41%, a11y fixes −30%, new UI −18%
- ~10% cheaper (9.6%); input tokens −18%, output −11%
- About 9 Storybook lookups per run replaced codebase exploration (−34%) → 12% fewer tool calls
- New UI: 160–210 fewer lines, ~3 fewer files → smaller PRs to review
- Click: the fullest Storybook (all docs + many stories + docgen) was no better than none. Focused "starter docs" (description, anatomy, minimal example): −32 to −35% time, −17 to −23% cost
Caveats (say briefly): one app, one model/effort, agent instructed to call the MCP at least once; measured MCP, not the new skills/CLI.
TODO: confirm with Steve / DX that these numbers are cleared for a public talk (the guide draft is marked internal).
-->

---

## Migration helpers

<SBWindow title="Terminal">

````md magic-move
```bash
npx storybook upgrade
```
```bash
npx storybook upgrade
┌  Storybook upgrade - v11.0.0
│
◇  1 project detected
│
●  Upgrading from 10.x to 11.0.0
│
◆  Updated package versions in package.json files
│
◆  Automigrations detected
│
◆  Select automigrations to run
│  ◻ skills
│  ◻ docgen-server
│  ◻ remove-experimental-review
│  ◻ remove-change-detection-flag
└
```
```bash
npx storybook automigrate
```
````

</SBWindow>

<!--
[15:30 · 45s] TODO: replace the output with a real 10 → 11 run.
Automigrations: `skills` installs the skills + AGENTS.md section; `docgen-server` renames the flag (now stable, on by default); review and change detection are always on, so their flags get removed.
Breaking changes:
- Test runner support ended → move to the Vitest addon
- Node ≥ 22.12, TypeScript ≥ 5.9, Vite ≥ 6.3, React ≥ 18, Angular ≥ 21
- `@storybook/nextjs` deprecated → `nextjs-vite`; CRA and Yarn PnP removed
- `storybook ai` removed → `storybook skills` / `storybook tools`
-->

---
layout: statement
---

# Still <SBGradientText>one terminal</SBGradientText>

<SBWindow title="Terminal" class="mt-10 text-left">

```bash
$ pnpm dev

  VITE v7  ready in 312 ms
  ➜  Local:     http://localhost:5173/
  ➜  Storybook: http://localhost:5173/__storybook/
  ➜  MCP:       http://localhost:5173/__storybook/mcp
```

</SBWindow>

<!--
[16:15 · 30s] Callback to slide "Starting 2 terminals": app, Storybook and your agent — one process.
TODO: match the real console output.
-->

---

# Try it

<div class="grid grid-cols-3 gap-6 mt-10">

<SBCallout title="Storybook 11">
Beta today<br/>Stable early December
</SBCallout>

<SBCallout title="Storybook as a Vite plugin">
Works with Storybook 11<br/><code>@storybook/vite-plugin</code>
</SBCallout>

<SBCallout title="Next from Labs">
Storybook devtools
</SBCallout>

</div>

<!--
[16:45 · 45s] TODO: add QR codes / links.
-->

---
layout: end
---

# Thank you!

<!--
[17:30 · 15s] Links & socials. Total ≈ 17:45, 0:15 buffer.
-->
