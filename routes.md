# Routes Map: MotionPrompt Library

## Routing Architecture
This project does **not** utilize a traditional router (like React Router, Next.js App Router, or Express). The entire root application is a Single Page Application (SPA) contained within `index.html` that manages views via DOM manipulation and Modals.

All routing is purely based on the static directory structure handled by the web server (Cloudflare Pages/Vercel).

## Static Route Inventory

| Route Path | File | Purpose | Auth Required |
|------------|------|---------|---------------|
| `/` | `/index.html` | The main library interface, grid, and search engine. | No |
| `/demos/[Demo_Name]/` | `/demos/[Demo_Name]/index.html` | Serves the specific interactive landing page demo. Often embedded via iframe, but can be visited directly. | No |
| `/demos/[Demo_Name]/prompt.md`| `/demos/[Demo_Name]/prompt.md`| The raw markdown text file containing the AI prompt. Fetched via JS, but can be visited directly. | No |

## Client-Side Virtual Routing (Modals)
The application fakes routing behavior for viewing prompts using a Modal overlay.
When a user clicks "View Prompt":
1. The URL does **not** change.
2. JavaScript prevents navigation.
3. The Modal overlay CSS class is changed to `.active`.
4. The content is dynamically swapped.

*Note: Deep linking directly to a specific prompt modal (e.g., `/?prompt=Iron_Man_Stark`) is currently not supported in the vanilla JS logic, but would be a recommended future upgrade.*
