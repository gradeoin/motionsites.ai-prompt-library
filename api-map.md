# API Inventory: MotionPrompt Library

## External APIs
There are **zero** external API calls (e.g., Stripe, Supabase, OpenAI, etc.) in the root orchestrator application. The application is completely self-contained.

## Internal APIs (File System Fetching)
The application acts as its own API by utilizing the browser's native `fetch()` API to read local static files asynchronously.

| Method | Route / Path | Purpose | Used By |
|--------|--------------|---------|---------|
| `GET` | `./demos/[Demo_Name]/prompt.md` | Retrieves the raw markdown text for a specific demo so it can be parsed by `marked.js` and rendered in the Modal. | The `click` event listener on `.open-prompt` buttons in `index.html`. |

## Error Handling
If the `fetch()` call fails (e.g., the `.md` file is missing or a 404 is returned by the CDN), the `catch` block in `index.html` intercepts the error and updates the Modal DOM to display a user-friendly error message:
`<p style="color: #ef4444;">Could not load prompt for [Title]. (Failed to fetch)</p>`
