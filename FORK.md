# RoadTracker fork setup

RoadTracker uses Archify's diagram engine and interactive viewer as the foundation for an independent product. This initial fork preserves the working upstream implementation; product changes can build on this baseline.

## Provenance

| Item | Value |
| --- | --- |
| Repository | <https://github.com/ajfero/roadtracker> |
| Upstream | <https://github.com/tt-a1i/archify> |
| Baseline branch | `main` |
| Baseline commit | `b25142deac4b19b293e45b6bf6d69351d3c8f3b6` |
| Upstream package version | `3.1.0` |
| Fork date | 2026-10-18 |

The original Git history and [MIT license](LICENSE) are retained, including the tt-a1i and Cocoon AI copyright notices. Package names, CLI commands, schemas, generated assets, and release metadata keep their upstream identities at this stage.

## Local development

Use Node.js 22 for development to match upstream's canonical test runtime; the engine supports Node.js 18 and later. From the repository root:

```sh
npm ci --ignore-scripts
npm --prefix archify ci --ignore-scripts
npm test
```

Render a self-contained HTML example:

```sh
ARCHIFY_UPDATE_CHECK_DISABLED=1 node archify/bin/archify.mjs deliver architecture \
  archify/examples/web-app.architecture.json /tmp/roadtracker-example.html --json
```

Open `/tmp/roadtracker-example.html` in a browser. The command also creates a delivery receipt beside it. The environment variable suppresses upstream update notifications while working on this fork. These commands use the local source and do not install an agent skill.

| Area | Location |
| --- | --- |
| CLI, renderers, schemas, and examples | `archify/` |
| Interactive viewer source | `viewer/` |
| Tests and build tooling | `test/`, `scripts/` |
| Upstream website and generated documentation | `website/`, `docs/` |
| Agent integrations | `integrations/` |

Use `npm run test:focus -- test/<name>.test.mjs` for a selected regression suite. See [CONTRIBUTING.md](CONTRIBUTING.md#choose-evidence-by-impact) for broader verification. Keep `README.md` and `README_EN.md` synchronized.

## Git workflow

The initial local checkout has `origin` pointing to `ajfero/roadtracker` and `upstream` pointing to `tt-a1i/archify`. Local `main` tracks `origin/main`; the default push remote is `origin`, and upstream pushes are disabled locally. These local settings are not transferred by a fresh clone.

Start product work on a branch:

```sh
git switch -c feature/describe-the-change
```

Inspect upstream changes before integrating them:

```sh
git fetch upstream
git log --oneline main..upstream/main
git diff main...upstream/main
```

Merge or cherry-pick selected changes on a separate branch, resolve conflicts, and run the relevant checks before integrating them into RoadTracker. Upstream contribution procedures apply when sending changes to Archify; RoadTracker work is directed by this project's owner.

## Automation and distribution

GitHub Actions are disabled on the new fork during initial setup. The inherited workflows include upstream release checks, Pages deployment, scheduled maintenance, and publishing paths. Adapt those workflows and their repository identities before enabling automation for RoadTracker.

The inherited website, release archive, update manifest, and installation instructions still describe Archify. A RoadTracker release needs its own identity and distribution configuration. This fork setup does not publish a website, release a package, or change installed agent skills.
