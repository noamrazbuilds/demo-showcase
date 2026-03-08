# HTML Showcase Template

Use this as the base HTML structure for generating showcase pages. Adapt the content sections, stats, and feature descriptions to match the specific app.

## Template Structure

The showcase page has these sections:

1. **Hero** — App name, tagline
2. **Stats bar** — 3-5 key metrics about the app
3. **Feature sections** — Step-by-step walkthrough with screenshots
4. **Capabilities grid** — What the app does (card layout)
5. **Key features grid** — Notable features beyond the core
6. **Footer** — Disclaimers, attribution

## Base HTML Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{APP_NAME}} - Demo Showcase</title>
  <style>
    :root {
      --bg: #fafafa;
      --card-bg: #ffffff;
      --text: #1a1a1a;
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --accent: #111827;
      --accent-light: #f3f4f6;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', system-ui, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
    }

    .container { max-width: 960px; margin: 0 auto; padding: 0 24px; }

    .hero { text-align: center; padding: 80px 24px 48px; }
    .hero h1 { font-size: 2.5rem; font-weight: 700; letter-spacing: -0.02em; margin-bottom: 16px; }
    .hero p { font-size: 1.15rem; color: var(--text-muted); max-width: 600px; margin: 0 auto; }

    .stats-bar {
      display: flex; justify-content: center; gap: 48px; padding: 32px 0;
      margin: 24px 0 48px; border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);
    }
    .stat { text-align: center; }
    .stat-number { font-size: 2rem; font-weight: 700; display: block; }
    .stat-label { font-size: 0.85rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.05em; }

    .feature { margin-bottom: 72px; }
    .feature-header { margin-bottom: 24px; }
    .feature-label {
      display: inline-block; font-size: 0.75rem; font-weight: 600;
      text-transform: uppercase; letter-spacing: 0.1em; color: var(--text-muted); margin-bottom: 8px;
    }
    .feature h2 { font-size: 1.5rem; font-weight: 600; margin-bottom: 8px; }
    .feature p { color: var(--text-muted); max-width: 640px; }

    .screenshot {
      border: 1px solid var(--border); border-radius: 12px; overflow: hidden;
      box-shadow: 0 4px 24px rgba(0, 0, 0, 0.06);
    }
    .screenshot img { width: 100%; display: block; }
    .screenshot-caption {
      padding: 12px 16px; background: var(--accent-light); font-size: 0.85rem;
      color: var(--text-muted); border-top: 1px solid var(--border);
    }

    .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }
    @media (max-width: 768px) {
      .two-col { grid-template-columns: 1fr; }
      .stats-bar { gap: 24px; flex-wrap: wrap; }
      .hero h1 { font-size: 1.75rem; }
    }

    .capabilities {
      display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 16px; margin-top: 32px; margin-bottom: 72px;
    }
    .capability { padding: 20px; background: var(--card-bg); border: 1px solid var(--border); border-radius: 8px; }
    .capability h3 { font-size: 0.95rem; font-weight: 600; margin-bottom: 4px; }
    .capability p { font-size: 0.85rem; color: var(--text-muted); }

    .footer {
      text-align: center; padding: 48px 24px;
      border-top: 1px solid var(--border); color: var(--text-muted); font-size: 0.85rem;
    }
  </style>
</head>
<body>

  <section class="hero">
    <h1>{{APP_NAME}}</h1>
    <p>{{APP_TAGLINE}}</p>
  </section>

  <div class="container">

    <div class="stats-bar">
      <!-- Repeat for 3-5 key stats -->
      <div class="stat">
        <span class="stat-number">{{STAT_VALUE}}</span>
        <span class="stat-label">{{STAT_LABEL}}</span>
      </div>
    </div>

    <!-- Feature sections: one per screenshot or screenshot group -->
    <div class="feature">
      <div class="feature-header">
        <span class="feature-label">{{STEP_LABEL}}</span>
        <h2>{{FEATURE_TITLE}}</h2>
        <p>{{FEATURE_DESCRIPTION}}</p>
      </div>
      <div class="screenshot">
        <img src="{{SCREENSHOT_FILE}}" alt="{{ALT_TEXT}}">
        <div class="screenshot-caption">{{CAPTION}}</div>
      </div>
    </div>

    <!-- For side-by-side screenshots use two-col -->
    <div class="feature">
      <div class="feature-header">
        <span class="feature-label">{{STEP_LABEL}}</span>
        <h2>{{FEATURE_TITLE}}</h2>
        <p>{{FEATURE_DESCRIPTION}}</p>
      </div>
      <div class="two-col">
        <div class="screenshot">
          <img src="{{LEFT_SCREENSHOT}}" alt="{{LEFT_ALT}}">
          <div class="screenshot-caption">{{LEFT_CAPTION}}</div>
        </div>
        <div class="screenshot">
          <img src="{{RIGHT_SCREENSHOT}}" alt="{{RIGHT_ALT}}">
          <div class="screenshot-caption">{{RIGHT_CAPTION}}</div>
        </div>
      </div>
    </div>

    <!-- Capabilities grid -->
    <h2 style="margin-bottom: 4px;">{{CAPABILITIES_HEADING}}</h2>
    <p style="color: var(--text-muted); margin-bottom: 0;">{{CAPABILITIES_SUBHEADING}}</p>
    <div class="capabilities">
      <div class="capability">
        <h3>{{CAPABILITY_TITLE}}</h3>
        <p>{{CAPABILITY_DESCRIPTION}}</p>
      </div>
      <!-- Repeat for each capability -->
    </div>

  </div>

  <div class="footer">
    <p>{{FOOTER_TEXT}}</p>
  </div>

</body>
</html>
```

## Design Notes

- The template uses a minimal, clean aesthetic with system fonts
- Color scheme is neutral (works for any app brand)
- Responsive — single column on mobile, two-col side-by-side on desktop
- Screenshots have rounded corners and subtle shadows for a polished look
- Stats bar provides quick at-a-glance metrics
- Feature sections follow a step-by-step narrative flow
- Capability cards work well for listing 6-18 features

## Customization Points

When generating the actual HTML, replace all `{{PLACEHOLDERS}}` with app-specific content:

- `{{APP_NAME}}` — The application name
- `{{APP_TAGLINE}}` — One-line description
- `{{STAT_VALUE}}` / `{{STAT_LABEL}}` — Key metrics (e.g., "18 Modules", "40+ Technologies")
- `{{STEP_LABEL}}` — Section label (e.g., "Step 1", "Results", "Features")
- `{{FEATURE_TITLE}}` — Section heading
- `{{FEATURE_DESCRIPTION}}` — 1-2 sentence description
- `{{SCREENSHOT_FILE}}` — Relative path to PNG
- `{{ALT_TEXT}}` — Accessible image description
- `{{CAPTION}}` — Screenshot caption
- `{{CAPABILITY_TITLE}}` / `{{CAPABILITY_DESCRIPTION}}` — Feature card content
- `{{FOOTER_TEXT}}` — Disclaimers or attribution
