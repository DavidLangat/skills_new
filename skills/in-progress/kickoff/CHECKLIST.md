# Coverage checklist

The **complete backlog** test. Every item ends in the roadmap as one or more epics, as N/A with a reason, or as "already exists". An item silently missing is a gap in the backlog.

Each item lists the questions that decide its scope; use them in the grilling step and again when drafting epics.

## Foundation

1. **Repo and tooling**: language, framework, package manager, monorepo or not, linting, formatting, type checking, pre-commit hooks.
2. **CI**: what runs on every push and pull request (lint, typecheck, tests, build), branch protection.
3. **Environments and config**: local, staging, production; how secrets and config reach each; seed data for local and staging.
4. **Staging deploy pipeline**: every merge to the main branch deploys to staging, so each slice ships from day one.

## Product

5. **Users and roles**: who uses the system, what each role can see and do.
6. **Auth**: sign-up, sign-in, sign-out, password reset or passwordless, social login, sessions, multi-factor, account deletion.
7. **Data model and migrations**: core entities and relations, migration tooling, data ownership per role.
8. **Domain features**: one epic per domain area, covering every user story in the description.
9. **Integrations**: payments, email, SMS, file storage, search, third-party APIs, webhooks in and out.
10. **Notifications**: which events notify whom, through which channels, with what preferences.
11. **Admin and support tooling**: back-office views, moderation, impersonation, manual fixes.

## Interface (UI projects)

12. **Design system**: implement the tokens and every component in `DESIGN.md`.
13. **States**: loading, empty, error, and offline states for every screen.
14. **Accessibility**: keyboard navigation, focus management, contrast, screen-reader labels, reduced motion.
15. **Responsiveness**: the breakpoints and devices supported.
16. **Internationalisation**: languages, locales, date, currency, and number formats.

## Quality

17. **Testing strategy**: which seams are tested, unit versus integration versus end-to-end, test data.
18. **Security**: input validation, authorisation checks on every endpoint, rate limiting, dependency scanning, secrets handling, OWASP top ten review.
19. **Performance**: target load, caching, pagination, indexes, bundle size budget.
20. **Privacy and compliance**: personal data inventory, retention, export and deletion requests, consent, any regulation the description names.

## Operations

21. **Infrastructure**: hosting, database, storage, queues, infrastructure as code, cost budget.
22. **Production deploy**: release process, zero-downtime deploys, database migrations in production, rollback.
23. **Observability**: structured logging, error tracking, metrics, uptime checks, alerts and who receives them.
24. **Backups and recovery**: backup schedule, restore drill, recovery time and recovery point targets.
25. **Analytics**: product events to track, dashboards.

## Launch

26. **Documentation**: README, local setup guide, architecture overview, API docs, runbooks for incidents.
27. **Launch checklist**: domain and DNS, TLS, legal pages (terms, privacy), production smoke test, go-live and post-launch monitoring.
