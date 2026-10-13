# Hosted API status

The public API origin exposes a plain-text health body and an OpenAPI document agents and integrators can fetch without console login.

## Sub-features

- `api-health` returns `OK` from `GET https://api.openconnector.dev/`.
- `api-openapi` serves `GET https://api.openconnector.dev/api/v1/spec.json` titled Open Connector Native API.
- `api-catalog` advertises the API via `https://openconnector.dev/.well-known/api-catalog`.

## How to get to it (user POV)

- Call the API status URL directly.
- Follow service-desc links from the marketing site's `Link` headers or `/.well-known/api-catalog`.
- Open the HTML service doc at `https://api.openconnector.dev/api/v1/`.

## Driving it with verify-openconnector

Preconditions:

- `verify-openconnector doctor` exited `0` (already covers health + OpenAPI).

- **Health.** `curl -sL https://api.openconnector.dev/` prints `OK` with HTTP 200. Save stdout to `$OPENCONNECTOR_VERIFY_EVIDENCE/features/api-status/health.txt`.
- **OpenAPI.** Run `verify-openconnector fetch https://api.openconnector.dev/api/v1/spec.json`. Body JSON `info.title` is `Open Connector Native API`.
- **Catalog.** Fetch `https://openconnector.dev/.well-known/api-catalog` and assert the linkset anchor points at `https://api.openconnector.dev/api/v1/`.
- **Proof.** Keep health text, OpenAPI snippet (`info` only), and catalog JSON in the feature evidence directory.

## Gotchas

- Response header `x-open-connector-release` is useful for doctor notes but is not a user-facing feature.
- Do not call authenticated `/api/v1` tool routes without a project key owned by the run.
- Console login (`app.openconnector.dev/login`) is a separate surface; a 200 login page does not prove API authorization.
