---
name: verify-openconnector
description: Drive the hosted Open Connector commercial product (marketing site, docs, API status, console login) the way a user does and capture proof. Use when verifying openconnector repo or commercial surface changes, or when an agent needs launch → doctor → drive → evidence → cleanup for openconnector.dev.
---

# verify-openconnector

Project-local verification skill for **openconnector** (commercial / Cloud / Enterprise product surfaces).

## Repo reality (read this first)

As of this skill's creation, `github.com/openconnector-dev/openconnector` is a **public placeholder** (README + CLA + LICENSE). It does **not** ship a local Console, server, or `pnpm dev` stack.

The primary user-facing commercial surface is the **hosted product**:

| Surface | Origin |
| ------- | ------ |
| Marketing site | `https://openconnector.dev` |
| Docs | `https://openconnector.dev/docs` (also `docs.openconnector.dev` → redirect) |
| Hosted API | `https://api.openconnector.dev` (`GET /` → `OK`) |
| Hosted console | `https://app.openconnector.dev` |

Do **not** copy community-only local launch (`pnpm dev` / Alchemy Stack) into this repo. Community verification belongs in `openconnector-community`. When commercial application source returns to this repo, extend Launch/Doctor here — do not invent a local stack that is not in the checkout.

Helper (executable):

```sh
.cursor/skills/verify-openconnector/scripts/verify-openconnector <command>
```

Put the helper on `PATH` for a run, or invoke it with an absolute path from the repo root.

## Launch

There is no long-lived local app process. Launch prepares an isolated verification workspace and confirms the helper can write evidence.

```sh
export OPENCONNECTOR_VERIFY_RUN_ID="manual-$(date -u +%Y%m%dT%H%M%SZ)"
# optional overrides:
# export OPENCONNECTOR_MARKETING_ORIGIN=https://openconnector.dev
# export OPENCONNECTOR_API_ORIGIN=https://api.openconnector.dev
# export OPENCONNECTOR_APP_ORIGIN=https://app.openconnector.dev

.cursor/skills/verify-openconnector/scripts/verify-openconnector launch
```

Ready means:

- `launch` prints `launch ok` and an `evidenceDir` under `/tmp/openconnector-verify-<runId>/evidence` (or `OPENCONNECTOR_VERIFY_EVIDENCE`)
- `doctor` (next section) exits `0`

Teardown is `cleanup` (below). Evidence is never deleted by cleanup.

## Doctor

Read-only check: are the hosted commercial surfaces worth driving?

```sh
.cursor/skills/verify-openconnector/scripts/verify-openconnector doctor
```

Doctor must pass all of:

- Marketing `/` contains `Open Connector`
- `/integrations` contains `Search toolkits`
- `/docs`, `/pricing`, `/enterprise` return HTTP 200 with expected markers
- `GET https://api.openconnector.dev/` body is exactly `OK`
- `GET …/api/v1/spec.json` contains `Open Connector Native API`
- `https://app.openconnector.dev/login` returns HTTP 200 with `open-connector`

Report lands at `$OPENCONNECTOR_VERIFY_EVIDENCE/doctor.txt`. If anything looks off, run doctor before driving again. Do not drive a host that doctor failed.

## Drive

Prefer HTTP + headless Chrome screenshots via the helper. Stable handles from the live site:

| Handle | Where |
| ------ | ----- |
| `aria-label="Primary navigation"` | Site chrome |
| `aria-label="Open Connector"` | Brand link |
| Nav links `Integrations`, `Skills`, `Pricing`, `Docs` | Header |
| CTA `Join waitlist` → `/#waitlist` | Header / hero |
| `aria-label="Search toolkits"` + placeholder `Search toolkits, e.g. GitHub, Slack, Google Drive` | Integrations directory |
| `aria-label="Toolkits"` | Toolkit list region |
| Integration detail routes `/integrations/<slug>` (e.g. `/integrations/github`) | Catalog |
| API status body `OK` | `api.openconnector.dev/` |

Recipes live in `features/`. Drive **one** mapped feature per proof unless the task requires broader coverage.

Example — integrations browse (mapped feature):

```sh
.cursor/skills/verify-openconnector/scripts/verify-openconnector drive-integrations
```

Browser (when you need interactive UI beyond the helper): use a **disposable** Chrome profile at the path printed by `launch` (`profileDir`). Prefer role/ARIA queries over coordinates. Never attach to the user's everyday browser profile.

Console auth mutations (signup, API keys, connections) require owned test credentials and are out of scope for the default unauthenticated map. Report them as unmet preconditions in the feature Gotchas rather than inventing credentials.

## Evidence

Proof standards:

- Exercise the real public user path (marketing/docs/API/console login page), not internal-only setters.
- Capture the action and the resulting state (list page + detail page, or form open + validation), not only a final screenshot.
- Verify side effects only when the journey has them (waitlist submit writes a server response — prefer dry validation unless the run owns the mailbox).
- Store artifacts under the run's `evidenceDir` (default `/tmp/openconnector-verify-<runId>/evidence/`). **Never commit evidence into the git checkout.**

Typical layout:

```
evidence/
  doctor.txt
  run.json                 # written next to evidence under work root
  http/…                   # fetch bodies/headers
  screenshots/…
  features/<feature-id>/…
```

Also copy durable proof into `/opt/cursor/artifacts/` when handing off a Cloud Agent walkthrough.

## Cleanup

```sh
.cursor/skills/verify-openconnector/scripts/verify-openconnector cleanup
```

Cleanup:

- Kills only a Chrome PID recorded in this run's pid file (never `pkill` by name)
- Removes the disposable Chrome profile directory
- **Keeps** `evidence/` and `run.json`

After cleanup, confirm evidence still exists (`cleanup` prints the path and lists it). A cleanup that deletes proof fails verification.

## Helpers

| Command | Purpose |
| ------- | ------- |
| `launch` | Create work/evidence/profile dirs |
| `doctor` | Hosted surface health |
| `fetch <path-or-url>` | GET + save body/headers |
| `screenshot <url> <out>` | Headless Chrome screenshot |
| `drive-integrations` | Prove integrations catalog → GitHub detail |
| `cleanup` | Tear down scratch; keep evidence |
| `paths` | Print JSON paths for the run |

## Feature map

See [`features/README.md`](features/README.md). Keep the map honest with `/maintain-verification-skill` as commercial surfaces change.
