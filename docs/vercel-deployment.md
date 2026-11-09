# Vercel deployment directories

This repository has separate frontend and backend Vercel projects. The root `vercel.json` is for the frontend only. Do not apply its `frontend/dist` output directory to the Express backend.

## Backend: med-broker-backend

In Vercel Project Settings → Build and Deployment:

- Root Directory: `backend`.
- Framework Preset: Express.
- Disable old Install Command, Build Command, and Output Directory overrides so `backend/vercel.json` supplies them.
- Install: `npm ci`; build: `npm run build` (generates Prisma client).
- Output Directory: leave unset. Express uses `backend/app.js`, which exports the app from `src/app.js`.

Keep existing production environment variables, including DATABASE_URL and JWT_SECRET. Do not put these values in source files or frontend variables. Redeploy the latest main commit after saving project settings.

Database migration/import commands are separate operational actions; the build configuration does not run them on every deployment.

## Frontend: med-broker-frontend

Keep its existing frontend configuration. If its Root Directory is repository root, the root `vercel.json` builds `frontend/dist`. If its Root Directory is `frontend`, use the Vite preset and `dist` output. The backend settings above must not be copied to the frontend project.

## Verification

After the backend deployment reports Ready, check `/health` for a connected database and `/api/medicines/demo` for the public read-only catalog. Verify sign-in with an existing account; never use a production payment as a deployment smoke test.
