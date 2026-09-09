# wpfda-backend

REST API for the Western Province Flying Disc Association (WPFDA) platform.
See `../docs/product-spec.md` for the full product specification.

## Stack

- Spring Boot 4 (Java 21), Maven
- PostgreSQL 18, Flyway migrations (`src/main/resources/db/migration`)
- Tests run against in-memory H2 (PostgreSQL mode) — no database needed to build

## Dev

```sh
# local database (credentials injected from 1Password, never stored in files)
op run --env-file=.env.op -- docker compose up -d

# run the API against it
op run --env-file=.env.op -- ./mvnw spring-boot:run

# build + tests (no DB required)
./mvnw verify
```

Health endpoint: `GET /actuator/health`.

Configuration is environment-driven: `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `PORT`.

## Docker

```sh
docker build -t wpfda-backend .
docker run -p 8080:8080 -e DB_URL=... -e DB_USERNAME=... -e DB_PASSWORD=... wpfda-backend
```

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) for the process and contributor licence terms,
and [AGENTS.md](AGENTS.md) for code conventions. A Dev Container is included, so opening
the repo in VS Code or Codespaces gives a ready environment with Java 21 and Maven.

Before opening a pull request, run the same checks CI runs:

```sh
./mvnw spotless:apply   # format Java sources
./mvnw -B verify        # format check, compile, tests
```

Security issues: see [SECURITY.md](SECURITY.md). Licence: proprietary, see [LICENSE](LICENSE).
