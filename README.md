# PulseLive

PulseLive is a small real-time polling app built with React + Vite and backed by Supabase. It supports two poll types (choice and Q&A), live voting with realtime updates, a presenter mode with a QR code for quick access, and basic poll management (draft → live → closed). It's intended for presenters, teachers, or events that need lightweight live audience interaction.

## Tech stack
- Language: TypeScript
- Framework / runtime: React 19 + Vite
- Notable libraries: @supabase/supabase-js, react-router-dom, qrcode.react

## Quick start

Prerequisites
- Node.js (recommend v18+)
- pnpm (recommended — this repo includes pnpm-lock.yaml and pnpm-workspace.yaml) — npm/yarn also supported

1. Install
```bash
pnpm install
# or
# npm install
```

2. Create environment file
```bash
cp .env.example .env
# then set:
# VITE_SUPABASE_URL=https://your-project.supabase.co
# VITE_SUPABASE_PUBLISHABLE_KEY=your-anon-or-publishable-key
```

3. Run in development
```bash
pnpm dev
# or
# npm run dev
```

4. Build / preview
```bash
pnpm build
pnpm preview
```

Deployment
- A `vercel.json` is present; you can deploy to Vercel or any static host that serves the Vite build.

## Environment variables
- `VITE_SUPABASE_URL` — Supabase project URL
- `VITE_SUPABASE_PUBLISHABLE_KEY` — Supabase anon / publishable key

These are referenced in `src/lib/supabase.ts`. If absent, the client is initialized with placeholder values and a console warning is shown.

## Database schema (inferred)
The app expects these tables/fields in the Supabase Postgres DB:

- `polls`
  - id (string/UUID)
  - title (string)
  - question (string)
  - type ('choice' | 'qa')
  - status ('draft' | 'live' | 'closed')
  - created_at (timestamp)

- `options`
  - id
  - poll_id
  - option_text
  - display_order

- `votes`
  - id
  - poll_id
  - option_id
  - created_at

- `questions` (for Q&A)
  - id
  - poll_id
  - question_text
  - upvotes
  - created_at

Realtime: the client subscribes to a Supabase channel named `poll-{id}` and listens for `postgres_changes` events to update polls/options/votes/questions live.

Note: ensure your Supabase policies and CORS permit the client to read/write the necessary tables from the browser, or route actions through a backend if you need stricter control.

## How it fits together
- `src/main.tsx` mounts the React app and router.
- `src/App.tsx` contains routes and most UI/logic: Dashboard, Create, Poll management, Present, Vote, Results.
- `src/lib/supabase.ts` initializes the Supabase client used across the app.
- styles live in `src/styles.css`; the app is a single-page React UI served by Vite.

## Project layout
```
.
├─ .env.example          # example env variables for Supabase
├─ index.html
├─ package.json          # scripts: dev, build, preview, lint
├─ pnpm-lock.yaml
├─ pnpm-workspace.yaml
├─ vercel.json
├─ tsconfig*.json
├─ vite.config.ts
└─ src/
   ├─ App.tsx            # main app, routes & UI
   ├─ main.tsx           # app bootstrap
   ├─ styles.css
   ├─ vite-env.d.ts
   └─ lib/
      └─ supabase.ts     # supabase client initializer
```

## Development notes & suggestions
- `App.tsx` currently holds most logic and UI; consider splitting into smaller components (`components/`, `hooks/`, `services/`) as the app grows.
- Tests are not present — add unit / integration tests if you plan to extend features.
- Review Supabase RLS policies for production use. Consider using server-side functions or an authenticated server role for sensitive operations.

## Troubleshooting
- "Cannot read from Supabase" — confirm `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY` are set in `.env` and the project is redeployed/restarted.
- Realtime issues — ensure database replication and realtime (`postgres_changes`) are enabled in your Supabase project and that your client key has access.

## Contributing
- Open issues and pull requests are welcome.
- If you make breaking changes to the DB schema, update this README and any migration scripts.

## License
No LICENSE file is present. Add a LICENSE file (MIT recommended) if you want a permissive open-source license.

---

If you'd like, I can also:
- Add a short example Supabase SQL migration to create the required tables (I can open a PR).
- Split `App.tsx` into smaller components and open a branch/PR with the refactor.
- Generate a minimal `CONTRIBUTING.md` and an `LICENSE` file.
