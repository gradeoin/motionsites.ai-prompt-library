# Architecture Map: MotionPrompt Library

## High-Level Architecture Diagram

```text
[ User / Browser ]
        │
        ▼
[ Global CDN (Cloudflare Pages / Vercel) ]
        │
        ▼
[ Root Orchestrator: index.html (Vanilla JS SPA) ]
        │
        ├─► (Search & Filter Logic)
        ├─► (IntersectionObserver Pagination)
        ├─► (RAM-Optimized Iframe Manager)
        └─► (Markdown Reader Modal)
                 │
                 ▼
[ Local Static File System ]
        │
        ├─► /demos/[Name]/index.html (Self-contained Demo Web App)
        └─► /demos/[Name]/prompt.md (Raw Text Prompt)
```

## Frontend (Orchestrator)
The root application relies solely on a single `index.html` file. It avoids modern bundlers (like Webpack or Vite) for the root application to maintain absolute simplicity and zero-build deployment.
- **Performance Architecture:** The site dynamically constructs the DOM as the user scrolls. Instead of standard lazy-loading, it implements an aggressive **destroy-and-rebuild** architecture for `<iframe>` elements to ensure low RAM consumption on user devices.

## Demos (Child Applications)
Each folder inside `/demos/` represents a completely independent web application. Most of these were built with Next.js, exported as static HTML (`next export`), and dropped into the folder. They run in total isolation inside the iframes.

## Infrastructure
- **Hosting:** Cloudflare Pages (Primary) / Vercel (Fallback).
- **Deployment Strategy:** Push-to-deploy. No build commands are executed by the hosting provider for the root directory. The provider simply syncs the static file tree to its edge nodes.
- **Caching:** The CDN aggressively caches all `.html`, `.css`, `.js`, and `.md` files at edge locations globally.

## Missing/Unnecessary Layers
Because this is a static showcase platform:
- **NO Backend API Layer** (Node, Python, Go, etc.)
- **NO Database Layer** (SQL, NoSQL, ORM)
- **NO Authentication Layer** (JWT, OAuth, Sessions)
- **NO State Management Library** (Redux, Zustand)
- **NO Queues or Background Workers**
