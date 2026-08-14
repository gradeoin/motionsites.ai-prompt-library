# Database Map: MotionPrompt Library

## Database Architecture
**UNKNOWN / NOT APPLICABLE** - This project does **not** use a traditional database (SQL, NoSQL, ORM, Supabase, Firebase, etc.).

## In-Memory Database Simulation
Data persistence is handled by a hardcoded Javascript array of Objects inside the `<script>` tag of `index.html`. This array acts as the read-only database for the application.

### `const demos` (Array of Objects)

| Field | Type | Purpose | Example |
|-------|------|---------|---------|
| `name` | String | Acts as the Primary Key and directory path reference. | `"Iron_Man_Stark"` |
| `title` | String | The human-readable display name. | `"Iron Man Stark Industries"` |
| `cat` | String | The filter category (e.g., SaaS, Agency, Portfolio). | `"Landing Page"` |
| `type` | String | Secondary classification tag. | `"Landing Page"` |
| `premium` | Boolean | (Optional) Flag to apply premium CSS styling. | `true` |

## Entity Relationships
None. The data structure is flat and read-only. Modifying the "database" requires developers to manually add or edit objects in the `index.html` source code and push the commit to GitHub.
