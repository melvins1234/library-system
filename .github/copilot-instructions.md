This repository contains a small multi-service library management system: a React + Vite front-end at `app/library-app` and a Laravel-based API at `services/library-app-api`.

Be concise and follow these local conventions when suggesting code changes or creating new files:

- Architecture summary
  - Front-end: `app/library-app` — React (TypeScript) + Vite. Key files: `src/main.tsx`, `src/App.tsx`, `package.json`, `vite.config.ts`.
  - Back-end: `services/library-app-api` — Laravel (PHP 8.2+, Composer). Key files: `app/Http/Controllers`, `routes/web.php`, `composer.json`.
  - Orchestration: `docker-compose.yaml` exposes a `mysql` service used by the Laravel app. Environment variables (DB_DATABASE, DB_PASSWORD) come from the environment or `.env`.

- Common workflows (explicit commands)
  - Front-end dev: from `app/library-app` run `npm install` then `npm run dev` (runs Vite). Build with `npm run build` and preview with `npm run preview`.
  - Back-end dev: from `services/library-app-api` use Composer and Artisan. Common steps:
    - `composer install` then copy `.env.example` to `.env` and set DB vars.
    - `php artisan migrate` to create tables (the repo includes migrations in `database/migrations`).
    - For local dev you can use `php artisan serve` or use the repo's docker-compose MySQL and run Artisan commands against it.
  - Full stack: use `docker-compose up -d` to start MySQL (the compose file only defines mysql). Start the Laravel server and Vite separately as above.

- Project-specific patterns and conventions
  - Laravel is configured to auto-migrate on post-create in composer scripts; migrations and seeders live under `database/`.
  - Front-end uses TypeScript and Vite; the `build` script runs `tsc -b` before `vite build` — preserve typebuild step when modifying build scripts.
  - ESLint is present at `app/library-app/eslint.config.js`; follow its rules (project uses TypeScript-aware linting config files `tsconfig.app.json`, `tsconfig.node.json`).

- Integration points to check when editing code
  - API routes: `services/library-app-api/routes/web.php` — changes here affect the front-end.
  - Eloquent models: `services/library-app-api/app/Models` — watch for mass-assignment and casts when returning JSON to the front-end.
  - Front-end API calls: search `app/library-app/src` for fetch/axios usages and ensure endpoint paths match `routes/web.php`.

- Testing & CI
  - Back-end: PHPUnit is set up (`phpunit.xml`) — run tests with `./vendor/bin/phpunit` from `services/library-app-api`.
  - Front-end: no tests defined in repo. If adding tests, follow Vite + Jest/Testing Library conventions and add scripts to `package.json`.

- When making suggestions, be concrete and repo-aware
  - Provide the exact file(s) to edit and a minimal patch (3-8 lines if possible).
  - If you change API contracts, update both `routes/web.php` and at least one front-end file that consumes it (e.g., `src/App.tsx`) as an example.
  - Follow existing code style: TypeScript + React functional components, Laravel controllers returning JSON responses or views in `resources/views`.

- Safety checks before proposing changes
  - Confirm DB column names by inspecting `database/migrations/*.php` before changing model attribute names.
  - Preserve Composer/post-install scripts in `services/library-app-api/composer.json` unless explicitly improving them.

If anything here is unclear or you want more detail on a specific area (build matrices, test coverage, typical API routes used by the UI), say which area and I'll expand this file with examples and small runnable edits.
