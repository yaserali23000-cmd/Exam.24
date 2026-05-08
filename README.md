# منصة الامتحانات الإلكترونية

An Arabic-language electronic exam platform built with TanStack Start and deployed on Netlify. Teachers can create exams with multiple question types, share them with students via a link, and view all student grades in a persistent dashboard.

## Features

- **Exam Builder** — Create exams with MCQ, true/false, and free-text questions. Configure per-question time limits, marks, and whether to show correct answers immediately.
- **Student Experience** — Students enter their name and group, then take the exam with a countdown timer per question. Anti-retake protection via localStorage.
- **Persistent Results** — All submissions are stored in a Netlify Postgres database. The teacher dashboard shows grades, averages, and pass rates.
- **Arabic RTL UI** — Full right-to-left layout with Cairo/Tajawal fonts and a dark theme.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | TanStack Start |
| Frontend | React 19, TanStack Router v1 |
| Build | Vite 7 |
| Styling | Tailwind CSS 4 + custom CSS variables |
| Database | Netlify Database (Postgres) via Drizzle ORM |
| Language | TypeScript 5.7 (strict mode) |
| Deployment | Netlify |

## Routes

| Path | Description |
|------|-------------|
| `/` | Exam creator |
| `/share/:examId` | Share page with exam link and stats |
| `/take/:examId` | Student exam page (login → take → results) |
| `/results/:examId` | Teacher grade dashboard |

## Running Locally

```bash
npm install
netlify dev
```

Requires the Netlify CLI and a linked Netlify site for database access.
