---
name: App Showcase
description: This skill should be used when the user asks to "create a demo showcase", "capture app screenshots", "generate demo page", "build showcase", "screenshot my app", "create visual demo", "create a product demo", "generate app walkthrough", or "show off my app". It covers autonomously exploring a web application, capturing screenshots at key states, and assembling them into a polished HTML showcase page with descriptions.
version: 0.1.0
---

# App Showcase

Generate polished visual demo showcases for web applications by autonomously exploring the app, capturing screenshots at key states, writing descriptions, and assembling multiple output formats.

## Workflow Overview

The showcase generation follows this sequence:

1. **Detect and start** the app (or accept a live URL)
2. **Set up** Playwright for headless capture
3. **Explore** the app autonomously to identify interesting pages, features, and states
4. **Capture** screenshots at each significant state using a Playwright script
5. **Describe** each screenshot with concise, engaging text
6. **Assemble** outputs: HTML showcase, Markdown with images, and optionally an animated GIF
7. **Clean up** temporary dependencies and stop the server if started

## Step 1: Detect and Start the App

Detect the app's start command and local URL. Read `package.json` and check for common patterns:

- `npm run dev` / `npm start` — check `scripts.dev` and `scripts.start`
- Look for framework indicators: Next.js (`next dev`), Vite (`vite`), Create React App, Remix, Nuxt, SvelteKit
- Detect the port from the start script or defaults (3000, 5173, 8080, 4321)
- If the user provides a URL directly (e.g., `https://example.com`), skip detection and use it

If the app is not already running, start it in the background and wait for the port to respond before proceeding.

## Step 2: Set Up Playwright

Check if Playwright is available. If not, install it temporarily:

```bash
npm install --no-save playwright 2>/dev/null
npx playwright install chromium
```

Clean up after capture completes by removing the temporary install.

## Step 3: Explore the App

Explore the app to identify what to screenshot. Use a two-phase approach:

**Phase A — Page discovery:** Navigate to the root URL. Read the page content and identify:
- Navigation links, menus, sidebar items
- Key interactive elements (forms, buttons, modals)
- Different page routes or views
- Authentication or onboarding flows

**Phase B — State identification:** For each interesting page or feature, identify distinct visual states worth capturing:
- Empty/default state vs. populated state
- Form inputs, validation states
- Loading/progress indicators
- Results, dashboards, data views
- Expanded/collapsed sections, modals, tooltips
- Error states if interesting

Target 6-10 screenshots that tell a clear story of what the app does and how it works.

## Step 4: Capture Screenshots

Write a Playwright capture script (`.mjs`) to navigate through identified states and save PNG screenshots. Key patterns:

- Use a 1280x800 viewport for consistent, web-friendly dimensions
- Add short delays (`300-500ms`) after interactions for render settling
- Name files sequentially: `01-homepage.png`, `02-feature-name.png`, etc.
- Use `page.waitForSelector()` for dynamic content
- Scroll to elements with `scrollIntoViewIfNeeded()` for below-fold content
- Handle SPAs — wait for route transitions to complete

Save all screenshots to a `demo-showcase/` directory in the project root.

For the capture script template, see `references/capture-script-template.md`.

## Step 5: Write Descriptions

For each screenshot, write two pieces of text:

1. **Alt text** (1 sentence) — Accessible description of what's visible in the screenshot
2. **Caption** (1-2 sentences) — Engaging description highlighting the feature or workflow step

Group screenshots into a narrative flow: setup, core features, results/output.

## Step 6: Assemble Outputs

### HTML Showcase (primary)

Generate a self-contained HTML file referencing the screenshots via relative paths. For the HTML template structure and styling, see `references/html-template.md`.

The HTML showcase includes:
- Hero section with app name and tagline
- Stats bar (key metrics about the app)
- Step-by-step walkthrough sections with screenshots
- Feature/capability grid
- Footer with disclaimers

### Markdown with Images

Generate a `demo-showcase/SHOWCASE.md` file with:
- Heading and app description
- Each screenshot as `![alt text](filename.png)` with caption below
- Feature list

### Animated GIF (optional)

If a GIF is requested, use Playwright's built-in video recording:
- Create the browser context with `recordVideo: { dir: 'demo-showcase/', size: { width: 1280, height: 800 } }`
- Navigate through key states with 1-2 second pauses between actions
- Close the context to finalize the `.webm` file
- Convert to GIF using `ffmpeg` if available: `ffmpeg -i video.webm -vf "fps=10,scale=960:-1" demo.gif`
- Keep under 15 seconds for reasonable file size
- If `ffmpeg` is not available, keep the `.webm` file and note this in the output

## Step 7: Clean Up

- Remove temporary Playwright install if one was performed
- Stop the dev server if one was started
- Report what was created and where

## Output Structure

```
project-root/
└── demo-showcase/
    ├── 01-homepage.png
    ├── 02-feature.png
    ├── ...
    ├── demo-showcase.html    # Main HTML showcase
    └── SHOWCASE.md           # Markdown version
```

## Important Guidelines

- Never modify the project's source code — only create files in `demo-showcase/`
- Auto-detect the app stack for relevant descriptions (e.g., "Built with Next.js and React")
- Keep descriptions factual and based on what's actually visible in screenshots
- Screenshots should tell a story — order matters
- Add `demo-showcase/` to `.gitignore` by default (append if exists, create if not)
- If Playwright capture fails, provide clear error messages and suggest manual alternatives

## Additional Resources

### Reference Files

- **`references/html-template.md`** — Complete HTML template with styling for the showcase page
- **`references/capture-script-template.md`** — Playwright script template for screenshot capture
