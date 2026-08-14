# Dependency Graph: MotionPrompt Library

## Core Dependencies (External)
1. **Google Fonts (Inter & Instrument Serif)**
   - **Type:** CSS Stylesheet
   - **Source:** `https://fonts.googleapis.com`
   - **Impact:** Critical for typography and visual branding.
2. **Marked.js**
   - **Type:** JavaScript Library
   - **Source:** `https://cdn.jsdelivr.net/npm/marked/marked.min.js`
   - **Impact:** High. Required to parse raw Markdown files into HTML. If this CDN goes down, the "View Prompt" modal will display raw markdown syntax instead of formatted text.

## Internal File Dependencies

```mermaid
graph TD;
    A[index.html] -->|Reads configuration from| B(const demos array)
    A -->|Renders iframes targeting| C(/demos/[Name]/index.html)
    A -->|Fetches text from| D(/demos/[Name]/prompt.md)
    C -->|Loads assets from| E(/demos/[Name]/_next/static/*)
```

## Critical Files (Do Not Modify Lightly)
1. **`index.html`**: The entire brain of the application. Modifying the `IntersectionObserver` logic heavily impacts browser memory performance. Modifying the `renderNextBatch()` logic impacts infinite scrolling.
2. **`/demos/*/prompt.md`**: These files must exactly match the `name` key defined in the `demos` array in `index.html`. If renamed, the fetch API will throw a 404 error when clicking "View Prompt".
