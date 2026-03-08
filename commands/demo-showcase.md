---
description: Generate a visual demo showcase with screenshots for the current app
argument-hint: [url-or-empty]
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, Agent
---

Generate a polished visual demo showcase for a web application. Capture screenshots autonomously, write descriptions, and assemble them into an HTML showcase page.

Use the **App Showcase** skill for detailed guidance on each step.

## Target URL

If an argument was provided (`$ARGUMENTS`), use it as the target URL.

If no argument was provided, auto-detect:
1. Read `package.json` to find the dev/start script and framework
2. Check if a dev server is already running on common ports (3000, 3001, 3002, 5173, 8080, 4321) by attempting a quick fetch
3. If not running, start the dev server in the background and wait for the port to respond
4. Construct the URL from localhost and the detected port

## Playwright Setup

Check if Playwright is available in the project:
```bash
node -e "require('playwright')" 2>/dev/null
```

If not available, install it temporarily (track this for cleanup):
```bash
npm install --no-save playwright && npx playwright install chromium
```

## Exploration Phase

Navigate to the target URL with Playwright (headless) and explore the app:

1. **Load the homepage** and take a screenshot
2. **Read the page structure** — identify navigation, key interactive elements, forms, distinct sections
3. **Plan 6-10 screenshots** that tell a coherent story:
   - Homepage / landing state
   - Key feature pages or views
   - Interactive states (forms filled, buttons clicked, modals open)
   - Results, outputs, or data views
   - Any unique or notable UI patterns
4. For each planned screenshot, decide:
   - What interactions to perform (clicks, typing, scrolling)
   - What selector to wait for
   - A descriptive filename

## Capture Phase

Write a Playwright capture script at `demo-showcase/capture.mjs` that navigates through all planned states and saves screenshots. Reference the capture script template from the App Showcase skill.

Run the script:
```bash
node demo-showcase/capture.mjs
```

Verify all screenshots were created:
```bash
ls -la demo-showcase/*.png
```

## Assembly Phase

For each captured screenshot, write:
- An **alt text** (1 sentence, accessible)
- A **caption** (1-2 sentences, engaging)
- A **section heading** and **description** for the HTML showcase

Then generate two output files:

### 1. HTML Showcase (`demo-showcase/demo-showcase.html`)

Use the HTML template from the App Showcase skill's references. Include:
- Hero with app name and tagline (detect from package.json name/description or page title)
- Stats bar with 3-5 key metrics about the app
- Feature sections with screenshots, ordered as a narrative walkthrough
- Capabilities grid listing what the app does
- Footer

### 2. Markdown Showcase (`demo-showcase/SHOWCASE.md`)

Generate a markdown version with:
```markdown
# App Name

Brief description.

## Screenshots

### Feature Name
![alt text](01-homepage.png)
Caption text.

### Next Feature
![alt text](02-feature.png)
Caption text.
```

## Cleanup Phase

1. Remove the capture script: `rm demo-showcase/capture.mjs`
2. Remove temporary Playwright if installed: `npm uninstall --no-save playwright`
3. Remove `demo-showcase/node_modules` and `demo-showcase/package*.json` if created
4. Add `demo-showcase/` to `.gitignore` if not already present (append, don't overwrite)
5. Report what was created:
   - Number of screenshots captured
   - Location of HTML showcase
   - Location of Markdown showcase
   - Reminder that `demo-showcase/` is gitignored (local only)

## Important Rules

- NEVER modify any existing project files except appending to `.gitignore`
- All output goes in the `demo-showcase/` directory
- If the dev server was started by this command, stop it at the end
- If Playwright install fails, explain the issue and suggest the user install it manually
- Keep descriptions factual — based on what's visible, not assumptions
- Screenshots should be 1280x800 viewport, PNG format
