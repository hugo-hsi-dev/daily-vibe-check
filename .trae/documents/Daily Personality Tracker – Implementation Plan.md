# Project Overview

Build a Nuxt 4 web app that tracks MBTI personality over time via daily micro-quiz, using Supabase for auth/database, Nuxt UI with playful theming, Nuxt Icons (Solar), and deployment via Dokploy. MVP focuses on auth, daily quiz (10 Q onboarding, then 3/day), streaks, personality calculation over a rolling 7-day window, and a basic dashboard.

## Milestones

0. DevOps Setup & CI/CD + Hello World
1. Foundation & Tooling
2. Supabase Setup (Auth + DB)
3. Domain Model & Migrations
4. Question Bank & Rotation Algorithm
5. Daily Quiz UI (Swipe)
6. Personality Calculation Engine (7‑day window)
7. Streak System
8. Dashboard & Personality Display
9. Theming, Icons, Animations
10. Deployment (Dokploy) & Env Management
11. QA: Tests & Observability
12. Phase 2: Historical Charts (@unovis/vue)
13. Progress Tracker Markdown

## Issue/PR Backlog (Step‑By‑Step Tasks)

### 0) DevOps Setup & CI/CD + Hello World
Pipelines
- [x] Add CI workflow: install (pnpm), lint, typecheck, build (GitHub Actions)
- [ ] Enable pnpm caching and Nuxt build cache (actions/cache)
- [ ] Add CD workflow: deploy to Dokploy on push to `main` (or tags)
- [ ] Configure secrets: Supabase keys, site URL, Dokploy token
Hello World
- [x] Create minimal Nuxt page and health endpoint (`/health`)
- [ ] Provision Dokploy app/service; set container port, health checks
- [ ] First deploy to staging domain; verify runtime and logs
Branching/Release
- [ ] Define branch strategy (PRs to `main`), conventional commits
- [ ] Optional: tag releases `v0.x` and generate changelog

Acceptance
- [ ] CI runs on PR and `main` with all checks passing
- [ ] CD deploys "Hello World" to staging via Dokploy
- [ ] Health endpoint responds 200; basic uptime verified

### 1) Foundation & Tooling
- [x] Confirm Nuxt 4 base, `@nuxt/ui`, `@nuxt/image` registered
- [x] Establish structure: `pages`, `components`, `composables`, `server/routes`
- [ ] ESLint + TypeScript strict; precommit formatting (lint-staged)
- [ ] Basic CI checks validated

Acceptance
- [ ] `pnpm build` passes locally and CI

### 2) Supabase Setup (Auth + DB)
- [ ] Provision Supabase project; retrieve anon/service keys
- [ ] Add Supabase client (plugin/composable) with secure env
- [ ] Configure email/password + social logins
- [ ] Protected routes and session handling

Acceptance
- [ ] Sign up/sign in/out works; redirects unauthenticated

### 3) Domain Model & Migrations
Tables
- [ ] `questions` (id, dimension, direction, text, active)
- [ ] `responses` (id, user_id, question_id, date, answer, strength)
- [ ] `daily_summaries` (user_id, date, per-dimension aggregates)
- [ ] `profiles_weekly` (user_id, week_start, type, percentages)
Indexes/Constraints
- [ ] Indexes for `user_id+date`, `question_id`; FKs and cascades

Acceptance
- [ ] SQL migrations applied; referential integrity verified

### 4) Question Bank & Rotation Algorithm
- [ ] Seed MBTI questions per dimension (mix positive/negative)
- [ ] Rotation picks least‑recent dimensions; avoids repeats
- [ ] Server timezone awareness for "today"

Acceptance
- [ ] Onboarding 10Q balanced; daily 3Q aligns with least‑recent data

### 5) Daily Quiz UI (Swipe)
- [ ] Build swipe interaction (touch + mouse/drag), right=Agree, left=Disagree
- [ ] Mobile‑first layout; playful UX and micro‑interactions
- [ ] Accessibility: keyboard fallback, ARIA labels
- [ ] Persist responses with optimistic UI and rollback

Acceptance
- [ ] Works on mobile/desktop; no duplicate submissions

### 6) Personality Calculation Engine (7‑day window)
- [ ] Map response+direction to spectrum contribution
- [ ] Compute rolling 7‑day aggregates per dimension (equal weight per day)
- [ ] Derive 4‑letter MBTI type; store daily summary and weekly profile

Acceptance
- [ ] Type and percentages consistent with fixtures

### 7) Streak System
- [ ] Track consecutive days; break on missing a day
- [ ] Milestone celebrations (7/30/60) animations

Acceptance
- [ ] Current and longest streak visible; milestones trigger animations

### 8) Dashboard & Personality Display
- [ ] Dashboard: current type, streak, total days, quick access to today’s quiz
- [ ] Personality card: 4‑letter type, full name, short description
- [ ] Percent breakdown per dimension; link to 16personalities.com
- [ ] Indicate changes from previous week

Acceptance
- [ ] Percentages accurate; weekly change indicator shown when applicable

### 9) Theming, Icons, Animations
- [ ] Nuxt UI custom theme tokens (colors, radius, motion)
- [ ] Solar icon set via Nuxt Icons
- [ ] Smooth transitions on quiz/swipe; micro‑interactions

Acceptance
- [ ] Consistent playful aesthetic; iconography reviewed

### 10) Deployment (Dokploy) & Env Management
- [ ] Dockerfile/Nitro preset for node-server; production build
- [ ] Dokploy app setup, secrets, server timezone config
- [ ] Staging + production environments; rollback strategy

Acceptance
- [ ] Successful deploys; timezone governs daily reset

### 11) QA: Tests & Observability
- [ ] Unit tests: rotation, calculation, streak
- [ ] E2E: onboarding 10Q → daily 3Q → dashboard
- [ ] Error handling/retry tests; client error boundaries
- [ ] Basic logging/metrics; uptime monitoring

Acceptance
- [ ] Coverage threshold met; CI green; health checks passing

### 12) Phase 2: Historical Charts (@unovis/vue)
- [ ] Integrate `@unovis/vue`
- [ ] Dimension score charts over time; week‑by‑week types
- [ ] Date range selector; perf & accessibility review

Acceptance
- [ ] Chart correctness and smooth mobile interactions

### 13) Project Plan Document
- [ ] Maintain `.trae/documents/Daily Personality Tracker – Implementation Plan.md` with checklist per issue/PR (Todo/In‑Progress/Done)
- [ ] Update on merge via CI or manual to reflect latest state

Acceptance
- [ ] Plan reflects latest merged status; easy to scan

## Data Model Draft

Dimensions: E/I, S/N, T/F, J/P
- Agree/Disagree mapped via question `direction` to ends of spectrum
- Daily summary: average per dimension per date
- 7‑day window: equal weight per day; derive percentages and dominant sides
- MBTI string: concatenate dominant sides across dimensions

## Technical Notes

- Server timezone defines "today"; compute start/end via server clock
- SSR routes for calculations/persistence; client composables for UI
- Use optimistic updates with rollback for quiz UX
- CI uses `pnpm i --frozen-lockfile` and caches `~/.pnpm-store`

## Definition of Done (per Issue/PR)

- [ ] Implementation complete
- [ ] Unit/E2E tests added/passing
- [ ] Accessibility and performance checked
- [ ] Brief docs updated
- [ ] CI green; reviewed and merged

## Next Step

On confirmation, I will:
- Finalize CD workflow and Dokploy deployment for staging
- Continue Supabase setup and begin implementing rotation algorithm
