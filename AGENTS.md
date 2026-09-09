# Repository guidance — wpfda-backend

This file is the single source of conventions for this repository. It is written for
both human contributors and coding assistants. `CLAUDE.md` and other assistant files
only import it. Product scope and data model live in the WPFDA product specification
(`docs/product-spec.md` in the parent `wpfda/` folder).

## What this repo is

The REST API for the Western Province Flying Disc Association platform: member register,
registrations and payments, clubs and teams, events, rosters and games, blog posts,
notifications and audit log. It serves the `wpfda-frontend` SPA and is the security
boundary for all three surfaces (public, club portal, admin portal).

## Layout

- Base package `za.org.wpfda.backend`. Organise by domain feature, not by layer:
  `member/`, `club/`, `event/`, `registration/`, … each holding its entity, repository,
  service and controller. Shared plumbing goes in `common/`.
- `src/main/resources/application.yaml` — configuration, environment-driven
  (`DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `PORT`). No profiles with secrets.
- `src/main/resources/db/migration/` — Flyway migrations `V<n>__<description>.sql`.
  Hibernate runs with `ddl-auto: validate`; the schema is owned by migrations.
- `src/test/` mirrors `src/main/`. Tests run on H2 in PostgreSQL mode, configured in
  `src/test/resources/application.yaml`.

## Conventions

- Java 21, Spring Boot 4. Prefer constructor injection, `record`s for DTOs, and
  `Optional` at API boundaries rather than `null`.
- Controllers are thin: validate input, call a service, map to a response DTO. Business
  rules (good standing, roster locks, import matching) live in services and are unit
  tested there.
- Never expose JPA entities from controllers. Every endpoint has an explicit request and
  response type.
- Authorisation is enforced in the API, not the frontend. Every non-public endpoint
  declares the roles that may call it. Club-facing member search returns name, member
  number and standing only (POPIA); do not widen it without a spec change.
- Every write to the member register, registrations or payments produces an
  `audit_log` row. Exports and outbound emails are logged.
- Formatting is google-java-format via Spotless. Run `./mvnw spotless:apply` before
  committing; CI fails on unformatted code.
- Prefer Spring's built-ins and dependencies already in the POM. A new dependency needs
  a clear reason in the PR description.
- Never commit secrets. Local credentials are injected with `op run --env-file=.env.op`.
  `.env.op` holds 1Password references only and is safe to commit.

## Tests and pull requests

- JUnit 5 with Spring Boot test slices. Service logic gets plain unit tests;
  controllers get `@WebMvcTest`; repositories and migrations get `@DataJpaTest` on H2.
  Keep full `@SpringBootTest` runs to a minimum.
- Before opening a PR run `./mvnw spotless:apply` then `./mvnw -B verify`. Both must
  pass. CI runs the same.
- Commits: imperative subject ≤ 72 characters, body explains why. Small, cohesive
  changes targeting `main`.
- PRs: describe the problem, the resulting behaviour and how you validated it. Include
  migration notes for schema changes. Link the issue. See `CONTRIBUTING.md`.

## Do not

- Do not edit an existing Flyway migration once it is on `main`; add a new one.
- Do not switch `ddl-auto` away from `validate`, even in tests.
- Do not run `docker compose up`, start the server, or run migrations against a real
  database as part of a code change unless explicitly asked; `./mvnw verify` needs
  neither.
- Do not change the Dockerfile base images without discussion.
