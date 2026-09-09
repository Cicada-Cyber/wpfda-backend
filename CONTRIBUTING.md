# Contributing to wpfda-backend

Thank you for helping build the WPFDA platform. This repository is the Spring Boot REST
API; the web app lives in [wpfda-frontend](https://github.com/Cicada-Cyber/wpfda-frontend).
Both repos follow the same contribution process, so once you know one you know the other.

## House rules

- Read [AGENTS.md](AGENTS.md) before changing code. It is the single source of conventions
  for this repo, for humans and coding assistants alike.
- Check [existing issues](https://github.com/Cicada-Cyber/wpfda-backend/issues) and
  [pull requests](https://github.com/Cicada-Cyber/wpfda-backend/pulls) before starting.
  Issues are not assigned or reserved; open a PR when you have something to show.
- When several PRs address the same issue, the one that solves it most simply and stays
  within scope is taken.
- The product is defined by the WPFDA product specification. Features outside that
  scope need an issue and a discussion first.
- Be kind. The [Code of Conduct](CODE_OF_CONDUCT.md) applies everywhere in this project.

## Contributor licence terms

This project is proprietary software owned by the Western Province Flying Disc
Association (see [LICENSE](LICENSE)). So that WPFDA can use, maintain and relicense the
platform, every contribution is accepted on these terms:

1. You assign to WPFDA all copyright in your contribution, or where assignment is not
   possible under your local law, you grant WPFDA a perpetual, worldwide, irrevocable,
   royalty-free licence to use, modify, distribute, sublicense and relicense it.
2. You confirm the contribution is your own original work, or that you have the right to
   submit it under these terms, and that it does not knowingly infringe anyone's rights.
3. You understand the project is not open source and you receive no licence to the
   Software beyond what LICENSE grants.

Opening a pull request and ticking the contributor-terms box in the PR template records
your agreement.

## Development setup

Two options:

1. **Dev Container** (VS Code or GitHub Codespaces). Open the repo and accept the prompt;
   Java 21 and Maven are preinstalled and dependencies are fetched on first open.
2. **Local**. Install a Java 21 JDK. The Maven wrapper (`./mvnw`) does the rest.

Tests run against in-memory H2, so no database is needed to build. To run the API against
a real Postgres, see the README (credentials come from 1Password, never from files).

## Making a pull request

1. Fork the repo and create a feature branch (`git checkout -b feat/member-import`).
2. Make small, cohesive commits with imperative subjects of at most 72 characters,
   for example `Add member register entity and migration`.
3. Run the pre-PR checklist below and make sure everything passes.
4. Push and open a PR against `main`. Tick **Allow edits from maintainers**.
5. Fill in the PR template: the problem, the resulting behaviour, how you validated it,
   and migration notes for any schema change. Link the issue with `Fixes #123` where one
   exists.
6. Wait for GitHub checks to go green and keep the branch up to date with `main`, then
   request a review.

### Pre-PR checklist

Run these locally before every PR. CI runs the same commands.

```sh
./mvnw spotless:apply   # format Java sources (google-java-format)
./mvnw -B verify        # format check, compile, tests
```

## Reporting security issues

Do not open a public issue. See [SECURITY.md](SECURITY.md).
