# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
Versions are bumped automatically from [Conventional Commits](https://www.conventionalcommits.org/)
on merge to `main` (see [README — Versioning](./README.md#versioning--releases)).

## [Unreleased]

### Added

- Project logo and favicon (`assets/logo.svg`, `assets/favicon.svg`) — an LED
  scoreboard badge in the Brazil palette; the favicon is embedded in
  `LiveScore.html` as a data URI to keep the app a single self-contained file.
- `<meta name="version">` embedded in `LiveScore.html` as the in-repo version
  marker, updated automatically by the release workflow.
- Configurable match duration: **−1 min / +1 min** stepper (active only while
  the clock is stopped); `Reset` returns to the configured duration.
- CI/CD pipeline (GitHub Actions):
  - `ci.yml` — HTMLHint, embedded-JS syntax check, a self-contained guard
    (PRD R1) and required-structure guards (PRD R2/R5) on every PR.
  - `release.yml` — SemVer bump from conventional commits, embedded-version
    update, git tag and GitHub Release.
  - `pages.yml` — deploys the scoreboard as a live demo on GitHub Pages.
- `.github/dependabot.yml` — weekly updates for GitHub Actions.
- `.github/pull_request_template.md` — standardized PR checklist.
- `.editorconfig`, `.gitignore`, `.htmlhintrc`, `README.md`, `CHANGELOG.md`.

### Changed

- Refactored `LiveScore.html` toward clean code with no behavior change:
  unified `fault` → `foul` naming; cached DOM lookups; collapsed the eight
  near-identical goal/foul handlers into parameterized `adjustGoal` / `adjustFoul`
  dispatch; single shared audio envelope (`tone`) replacing `beep`/`sweep`;
  encapsulated leaderboard persistence in a `leaderboardStore` module;
  promisified the confirm dialog; named constants and clearly ordered sections.
- Removed dead/redundant CSS and inline styles.
