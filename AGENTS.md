# AGENTS.md — guidance for Codex
## Project setup
* This is a mono-repo: `/web` for Next.js front-end, `/api` for NestJS back-end.
* `npm run dev` spins both with `concurrently`.
* Tests: `npm run test` in each sub-package.
## Coding style
* Use TypeScript, Prettier, ESLint (Airbnb).
* Use Prisma for DB layer.
## Tasks you can run
* `npm run generate` to create Prisma types.
* `npm run migrate` to apply migrations.
