# Playwright Capture Script Template

Use this as the base template for generating capture scripts. Adapt the navigation steps, selectors, and screenshot names to match the specific app being captured.

## Base Template

```javascript
import { chromium } from 'playwright';

const SCREENSHOT_DIR = new URL('./', import.meta.url).pathname;
const BASE_URL = '{{BASE_URL}}';

async function delay(ms) {
  return new Promise(r => setTimeout(r, ms));
}

async function main() {
  const browser = await chromium.launch();
  const page = await browser.newPage({ viewport: { width: 1280, height: 800 } });

  // 1. Homepage / landing page
  await page.goto(BASE_URL);
  await page.waitForSelector('{{MAIN_SELECTOR}}');
  await delay(500);
  await page.screenshot({ path: `${SCREENSHOT_DIR}01-homepage.png` });
  console.log('1/N Homepage captured');

  // 2-N. Additional states — adapt per app
  // Examples of common capture patterns below

  await browser.close();
  console.log('\\nAll screenshots saved to:', SCREENSHOT_DIR);
}

main().catch(e => { console.error(e); process.exit(1); });
```

## Common Capture Patterns

### Click and capture

```javascript
await page.click('text=Feature Name');
await delay(300);
await page.screenshot({ path: `${SCREENSHOT_DIR}02-feature.png` });
```

### Fill form and capture

```javascript
await page.fill('input[placeholder*="Enter"]', 'example value');
await page.click('text=Submit');
await delay(500);
await page.screenshot({ path: `${SCREENSHOT_DIR}03-form-filled.png` });
```

### Wait for async content

```javascript
await page.click('button:has-text("Load")');
await page.waitForSelector('.results', { timeout: 60000 });
await delay(1000);
await page.screenshot({ path: `${SCREENSHOT_DIR}04-results.png` });
```

### Scroll to section

```javascript
const section = page.locator('text=Section Heading');
if (await section.count() > 0) {
  await section.scrollIntoViewIfNeeded();
  await delay(300);
}
await page.screenshot({ path: `${SCREENSHOT_DIR}05-section.png` });
```

### Expand/collapse UI element

```javascript
await page.click('[data-testid="expand-button"]');
await delay(300);
await page.screenshot({ path: `${SCREENSHOT_DIR}06-expanded.png` });
```

### Tab or mode switching

```javascript
await page.click('text=Tab Name');
await delay(300);
await page.screenshot({ path: `${SCREENSHOT_DIR}07-tab-view.png` });
```

### Capture with long-running operation

```javascript
await page.click('button:has-text("Start")');
// Wait for progress indicator
await delay(3000);
await page.screenshot({ path: `${SCREENSHOT_DIR}08-in-progress.png` });
// Wait for completion
await page.waitForSelector('text=Complete', { timeout: 120000 });
await delay(1000);
await page.screenshot({ path: `${SCREENSHOT_DIR}09-complete.png` });
```

## Key Guidelines

- Always use `await delay(300-500)` after interactions for render settling
- Use `page.waitForSelector()` for dynamic content instead of fixed delays
- Name screenshots sequentially with descriptive names
- Set `timeout: 120000` for long operations (scans, builds, API calls)
- Use `page.locator()` for more reliable element targeting
- Capture at 1280x800 viewport for consistent web-friendly dimensions
- Handle cases where elements may not exist: check `await locator.count() > 0`
