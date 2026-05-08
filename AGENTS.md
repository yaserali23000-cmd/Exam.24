# AGENTS.md

## Project Overview

An Arabic RTL electronic exam platform. Teachers build exams at `/`, share them via URL, and students take them at `/take/:examId`. Results are stored in Netlify Postgres and visible at `/results/:examId`.

## Directory Structure

```
db/
  index.ts          # Drizzle client (netlify-db adapter)
  schema.ts         # exams + results tables
drizzle.config.ts   # points migrations to netlify/database/migrations/
netlify/database/migrations/   # auto-applied SQL migrations
src/
  server/
    exams.functions.ts   # TanStack server functions (createExam, getExam, submitResult, getExamResults)
  routes/
    __root.tsx           # HTML shell, Arabic RTL, Google Fonts
    index.tsx            # Exam builder (teacher)
    share.$examId.tsx    # Post-creation share page
    take.$examId.tsx     # Student exam flow (login → exam → results)
    results.$examId.tsx  # Teacher grade dashboard
  styles.css             # All CSS — dark theme, RTL, custom properties
  router.tsx             # TanStack Router setup
```

## Key Decisions

- **Database**: Netlify Postgres via `drizzle-orm@beta` + `drizzle-kit@beta` — required because the netlify-db adapter only exists on the beta release line.
- **Question data stored as JSONB**: The `questions` column on `exams` and `answers` on `results` are `jsonb` to avoid a separate questions table while keeping data queryable.
- **Anti-retake**: localStorage key `exam_done_<examId>` is set after submission. This is a lightweight UX guard, not a security measure.
- **No authentication**: The teacher dashboard is publicly accessible by URL. The exam ID is the only access control — a numeric serial from the DB.
- **Server functions** (not API routes): All DB access goes through `createServerFn` in `src/server/exams.functions.ts`, called directly from loaders and components.

## Coding Conventions

- Arabic string literals inline in JSX — no i18n library.
- CSS via a single `styles.css` file with CSS custom properties (`--accent`, `--green`, etc.) — not Tailwind utilities for the exam UI.
- TypeScript strict mode; `.js` extensions in server-side imports.
- Route files named `<segment>.$<param>.tsx` following TanStack file-based routing.

## Database Schema Changes

Always update `db/schema.ts` then run `npx drizzle-kit generate` to produce a migration. Never edit migration files after they are applied.
