# Codebase Memory: MotionPrompt Library

## Project Overview
**Name:** MotionPrompt AI-Powered Landing Page Demos
**Purpose:** A curated repository and showcase platform of 73+ high-fidelity, production-ready landing page demos. Each demo is built using modern web techniques (Next.js, Tailwind, WebGL, Framer Motion) and generated via specialized AI prompts.
**Business Purpose:** Solves the problem of demonstrating AI capabilities in generating premium, interactive UI/UX patterns (glassmorphism, scroll-driven animations). Serves as a portfolio and utility tool for designers and developers to browse demos and instantly copy the exact AI prompts used to generate them.

## Tech Stack
- **Frontend Framework:** Vanilla HTML/CSS/JavaScript (for the main directory site), Next.js/React/Tailwind (for the individual generated demos).
- **Backend Framework:** None (Statically Hosted).
- **Database:** None (Data is hardcoded in an in-memory JS array).
- **Authentication:** None.
- **State Management:** Vanilla JS variables (`activeFilter`, `searchQuery`, `currentRenderIndex`, `filteredDemos`).
- **Styling:** Vanilla CSS (Main site), Tailwind CSS (Demos).
- **Infrastructure:** Vercel / Cloudflare Pages.
- **Third-Party Libraries:** `marked.js` (loaded via CDN for parsing Markdown on the fly).

## Repository Structure
```text
/
├── index.html           # The core application and routing engine (SPA)
├── demos/               # Contains 73 individual demo projects
│   └── [Demo_Name]/
│       ├── index.html   # The actual rendered landing page demo
│       └── prompt.md    # The AI prompt used to generate this demo
├── prompts/             # Legacy storage for free prompts
├── Pro prompts/         # Legacy storage for premium prompts
├── assets/              # Global static assets (images, fonts)
├── package.json         # Project metadata
└── vercel.json          # Deployment configuration
```

## System Architecture
The project operates as a two-tier static system:
1. **The Orchestrator (`index.html`)**: A lightweight, high-performance Vanilla JS Single Page Application (SPA). It manages search, filtering, infinite-scroll pagination, and a dynamic modal system to parse Markdown. It embeds the demos using lazy-loaded `<iframe>` elements that are actively garbage-collected when scrolled out of view to preserve RAM.
2. **The Demos (`/demos/*`)**: Independent, self-contained web applications (often exported Next.js builds) served as static files.

## Routing Map
See `routes.md` for a detailed breakdown. Routing is purely directory-based static hosting.

## Frontend Architecture
- **State Management:** Handled entirely within a single `<script>` block in `index.html`. 
- **Performance Optimization:** Implements custom `IntersectionObserver` logic to aggressively unload `<iframe>` DOM nodes when they leave the viewport. This prevents massive memory leaks when browsing 70+ heavy WebGL/React demos.
- **Markdown Parsing:** Fetches raw `.md` text via the Fetch API and converts it to HTML using `marked.js` inside a custom-built Glassmorphic Modal.

## Backend / Database Architecture
- **Backend:** N/A
- **Database:** See `database-map.md`. The database is a hardcoded array of JSON objects inside `index.html`.

## Authentication Flow
- **Authentication:** N/A (Publicly accessible).

## API Inventory
- **External APIs:** None.
- **Internal APIs:** See `api-map.md`. Uses `fetch()` to retrieve static `.md` files.

## Data Flow Diagrams
**User views a prompt:**
User clicks "View Prompt" ↓ JS Event Listener captures `data-demo` attribute ↓ Opens Modal UI ↓ `fetch('./demos/[name]/prompt.md')` ↓ Raw text returned ↓ `marked.parse(text)` ↓ Injects HTML into DOM Modal ↓ User clicks "Copy" ↓ `navigator.clipboard.writeText()` ↓ UI Update ("Copied!").

## Environment Variables & Integrations
- **Environment Variables:** None required.
- **Integrations:** Cloudflare Pages / Vercel (Auto-deploy on Git push).

## Feature Inventory
1. **Live Grid:** A responsive CSS grid displaying 73 demos.
2. **Infinite Scroll Pagination:** Loads 12 demos at a time based on scroll intersection.
3. **Debounced Search:** Filters the in-memory array by title/category with a 300ms delay.
4. **Category Filtering:** Filter buttons (SaaS, Portfolio, Web3, etc.).
5. **Dynamic Markdown Reader:** A custom modal that parses and styles Markdown files on the fly.
6. **Iframe Memory Management:** Actively destroys hidden iframes to save RAM.

## Development Workflow & Deployment Process
- **Workflow:** Modifying `index.html` handles the core library. Adding a new demo requires creating a new folder in `/demos/`, pasting the `index.html` build and the `prompt.md`, and adding a new object to the `demos` array in the root `index.html`.
- **Deployment:** Git push to the `main` branch. Cloudflare Pages or Vercel detects the change, clones the repo, and pushes the static assets to their global CDN edge network. No build step is required for the root project.

## Known Risks & Future Recommendations
- **Risk:** Repository size is approaching 1GB due to heavy video/3D assets inside the `demos/` folder. This may eventually exceed GitHub's repository limits.
- **Recommendation:** Offload heavy video assets (MP4s/WebMs) to a dedicated object storage bucket (e.g., AWS S3, Cloudflare R2) and link to them absolutely, rather than storing them in the Git repository.
