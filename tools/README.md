# SPA Tools

This repository contains standalone single-page applications. Each application must be a single HTML file that contains the entire application.

## Requirements

- Keep each application self-contained in one `.html` file.
- Embed all markup, CSS, and JavaScript in that file.
- Do not require external files, tools, libraries, scripts, stylesheets, or other dependencies.
- Keep any assets needed by an application inside its HTML file as well.
- Build mobile-first: include a viewport meta tag, make the layout work on narrow screens, and enhance it for larger screens.
- Ensure content fits without unintended horizontal scrolling and controls are comfortable to use by touch.
- Make every change in a way that preserves the standalone, self-contained nature of the application.

## Styling and visual consistency

Use the same visual foundation for every new page so the tools feel like one collection:

- Start with a light, warm-gray page background, white surfaces, dark primary text, muted secondary text, and subtle borders.
- Use a system sans-serif font stack and define shared colors as CSS custom properties in `:root`. Use the default palette below consistently across pages; reserve other colors for semantic states (such as errors) or task-specific visualizations (such as wheel segments), not a separate page accent.
- Keep content centered with fluid page gutters, readable line lengths, clear heading hierarchy, and consistent spacing. Use 14px radii for primary cards and panels; use about 9px for standard form controls, allowing text areas up to 11px. Keep shadows restrained, and reserve pill shapes for prominent actions and larger radii for dialogs.
- Style buttons and form controls consistently, with visible keyboard focus, sufficient text contrast, and touch targets at least 44px high.
- Build mobile-first with fluid typography and layouts. Add breakpoints only where the content needs them, and verify there is no horizontal scrolling at narrow widths.

Use this base in each standalone HTML file and extend it only when the page needs additional tokens:

```css
:root {
  color-scheme: light;
  --ink: #20242b;
  --muted: #59636f;
  --line: #e0e3e8;
  --paper: #fff;
  --canvas: #f5f4f0;
  --accent: #3559e8;
  --accent-strong: #2343c7;
  --font: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;

  font-family: var(--font);
  color: var(--ink);
  background: var(--canvas);
}

* {
  box-sizing: border-box;
}

body {
  min-width: 320px;
  min-height: 100vh;
  margin: 0;
}
```
