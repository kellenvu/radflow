# sv

Everything you need to build a Svelte project, powered by [`sv`](https://github.com/sveltejs/cli).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```sh
# create a new project in the current directory
npx sv create

# create a new project in my-app
npx sv create my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```sh
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```sh
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

# RadFlow PRD

## Overview
RadFlow is a lightweight, interactive dashboard designed for radiology residents to track their daily workflow. It consolidates studies, conferences, and learning activities into a clear timeline view, helping residents monitor training progress, review key diagnoses, and reflect on time allocation.

## Goals
- Provide a simple daily timeline of studies and conferences.
- Track top diagnoses and recurring case patterns.
- Visualize activity (time spent on studies, conferences, learning).
- Support quick feedback loops with summaries and learning pearls.
- Keep UI clean, fast, and easy to use for quick reference.

## Technical
- Framework: **SvelteKit**
- Hosting: **GitHub Pages**
- Output: **Static site** (using adapter-static)
- Styling: **Tailwind CSS** (minimal, no extra plugins initially)
- Tooling: **Prettier** for formatting
