<p align="center">
  <img src="assets/logo.svg" alt="LiveScore" width="160" />
</p>

<h1 align="center">LiveScore</h1>

<p align="center">
  A digital LED <strong>football scoreboard</strong> in a single, dependency-free HTML file.<br/>
  <em>Built for a baby-shower friendly match</em> — goals, fouls, timer, sounds and an accumulated leaderboard, in the colors of Brazil.
</p>

<p align="center">
  <a href="https://github.com/HectorIFC/LiveScore/actions/workflows/ci.yml"><img src="https://github.com/HectorIFC/LiveScore/actions/workflows/ci.yml/badge.svg" alt="CI" /></a>
  <a href="https://github.com/HectorIFC/LiveScore/releases"><img src="https://img.shields.io/github/v/release/HectorIFC/LiveScore?sort=semver" alt="Release" /></a>
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License: MIT" />
  <img src="https://img.shields.io/badge/i18n-PT%20%7C%20EN%20%7C%20ES-7fd7ff" alt="Languages" />
</p>

## About

**LiveScore** reproduces the experience of a physical LED scoreboard, entirely in
one `.html` file with no build step and no external dependencies. It is operated
by one person (the "marker") while the public watches the screen.

### Principles

- **Single file** — HTML + CSS + JS embedded; works offline, even emailed as one file
- **No scrollbar, ever** — fits 100% of the viewport on any device
- **Responsive** — Smart TV, laptop and phone, landscape and portrait
- **Brazil palette** — green, yellow and blue, with the characteristic red LED digits
- **Multi-language** — Portuguese (default), English and Spanish, switched live
- **Synthesized sound** — every action plays a distinct sound via the Web Audio API, no audio files

## Features

- ⏱ **Timer** — countdown with Start / Pause / Reset, configurable duration
  (**−1 / +1 min** while stopped), manual period advance, and an end-of-match sound at `00:00`
- ⚽ **Goals** — +1 / −1 per team, each team with its own sound
- 🟨 **Fouls** — +1 / −1 per team (SET/FALTAS), with an alert sound
- 🏆 **Leaderboard** — each finished match accumulates goals by team code,
  sorted high→low, persisted in `localStorage`, with a clear button
- 🌐 **i18n** — instant PT / EN / ES switching
- 📺 **Clean mode** — hide the controls for a distraction-free presentation on a TV

## Run it

It is a standalone file — just open it:

```bash
open LiveScore.html        # macOS
# or double-click the file, or drag it into any browser
```

**Live demo:** <https://hectorifc.github.io/LiveScore/> (published by the Pages workflow).

## Tech & constraints

No framework, no bundler, no package manager. The whole app is `LiveScore.html`:
a `<style>` block, the markup, and one embedded `<script>`. The only external
dependencies in the repository are the GitHub Actions used by CI/CD (kept current
by Dependabot). This constraint is enforced in CI by a *self-contained guard*
that fails the build if any external script, stylesheet or `http(s)` asset is
referenced.

## Versioning & releases

Versions follow [Semantic Versioning](https://semver.org/) and are produced
automatically from [Conventional Commits](https://www.conventionalcommits.org/).
On merge to `main`, the **Release** workflow computes the next version from the
commits since the last tag:

| Commit type (PR title on squash-merge) | Bump  | Example         |
| -------------------------------------- | ----- | --------------- |
| `feat!:` / `BREAKING CHANGE:`          | major | 0.1.0 → 1.0.0   |
| `feat:`                                | minor | 0.0.1 → 0.1.0   |
| `fix:` / `build:` / `chore:`           | patch | 0.1.0 → 0.1.1   |

It then updates the embedded `<meta name="version">`, pushes the bump commit
(with `[skip ci]`), creates the git tag and a GitHub Release. The version visible
in the page and the GitHub release always match the tag.

## Workflows

| Workflow                                          | Trigger                       | Purpose                                                         |
| ------------------------------------------------- | ----------------------------- | -------------------------------------------------------------- |
| [`ci.yml`](.github/workflows/ci.yml)              | PRs to `main`, non-main pushes | HTMLHint, JS syntax, self-contained & required-structure guards |
| [`release.yml`](.github/workflows/release.yml)    | push to `main`                | SemVer bump, version update, git tag, GitHub Release           |
| [`pages.yml`](.github/workflows/pages.yml)        | push to `main`                | Deploy the live demo to GitHub Pages                           |

> **One-time setup for the live demo:** repo **Settings → Pages → Build and
> deployment → Source: GitHub Actions**. Until enabled, `pages.yml` fails
> harmlessly without affecting CI or Release.

## Contributing

Pull requests use the [template](.github/pull_request_template.md). Before opening
one, run `npx htmlhint LiveScore.html`, verify the app in a browser, and update
`CHANGELOG.md` under `[Unreleased]`.

## License

[MIT](./LICENSE).
