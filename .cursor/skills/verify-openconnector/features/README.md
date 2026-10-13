# Open Connector commercial verification map

Maintained source for verifying user-facing behavior of the **hosted** commercial Open Connector product. Read this index before driving, then use the matching feature file.

## Baseline preconditions

- This checkout has no local Console/server. Drive `https://openconnector.dev`, `https://api.openconnector.dev`, and `https://app.openconnector.dev`.
- Run `verify-openconnector launch` then `verify-openconnector doctor` and require exit code `0`.
- Set `OPENCONNECTOR_VERIFY_RUN_ID` so concurrent agents do not share `/tmp/openconnector-verify-*` workspaces.
- Use the disposable Chrome profile under the run's `profileDir` only. Never drive a shared interactive browser session.
- Do not submit the public waitlist with a real customer's email unless the run owns that mailbox.
- Do not authenticate into the hosted console without owned test credentials.

## Driving conventions

- Start every recipe after a passing doctor unless the feature file says otherwise.
- Prefer ARIA roles and accessible names (`Primary navigation`, `Search toolkits`, `Toolkits`) over CSS selectors or coordinates.
- Treat helper commands as literal. Keep quoted names and flags unchanged.
- HTTP proofs use `verify-openconnector fetch` / `drive-integrations`.
- Screenshots use `verify-openconnector screenshot`.
- Restore nothing on the public site (read-only journeys). Keep proof artifacts; cleanup must not remove them.

## Proof and skip reporting

- Capture the user action and the resulting state, not only the final screen.
- UI proof includes a screenshot with the Open Connector brand visible and a text proof (`proof.txt` or ARIA dump).
- HTTP proof includes status code, body markers, and saved body path.
- Record the feature ID and entry point with every artifact.
- Report an unreachable path with the attempted command and unmet precondition.
- Do not report a skipped entry point as verified through a different path.

## Feature entry contract

Each feature file starts with an H1 and one paragraph of user-visible behavior, then exactly these H2s:

1. `Sub-features`
2. `How to get to it (user POV)`
3. `Driving it with verify-openconnector`
4. `Gotchas`

## Features

- [Marketing home and waitlist](./marketing-home.md) — brand, hero, primary CTA, waitlist form (validation only by default).
- [Integrations catalog](./integrations-browse.md) — search toolkits and open a provider detail page.
- [Documentation entry](./docs-entry.md) — docs shell from marketing nav / docs origin.
- [Pricing and Enterprise](./pricing-enterprise.md) — commercial plan and enterprise preview pages.
- [Hosted API status](./api-status.md) — public API health and OpenAPI document.
