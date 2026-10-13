# Documentation entry

Docs give visitors the product explanation, quickstart, and self-hosting map without signing in.

## Sub-features

- `docs-from-nav` opens Docs from primary navigation.
- `docs-canonical` loads `https://openconnector.dev/docs` with the Open Connector docs shell.
- `docs-redirect` follows `https://docs.openconnector.dev/` to the canonical docs URL.

## How to get to it (user POV)

- Choose `Docs` in the marketing header.
- Choose Read the docs on the marketing home.
- Open `https://openconnector.dev/docs` or `https://docs.openconnector.dev/`.

## Driving it with verify-openconnector

Preconditions:

- `verify-openconnector doctor` exited `0`.

- **Fetch docs.** Run `verify-openconnector fetch /docs`. HTTP `200`. Body contains `Open Connector` and documentation map cues such as `Quickstart` or `Self-host`.
- **Screenshot.** Run `verify-openconnector screenshot /docs "$OPENCONNECTOR_VERIFY_EVIDENCE/features/docs-entry/docs.png"`. The docs title/branding is visible.
- **Redirect check.** `curl -sI https://docs.openconnector.dev/` shows a redirect to the marketing docs path. Record status and `location` in evidence; do not require a second full content proof beyond the canonical page.

## Gotchas

- Docs content is hosted on the marketing origin; do not assume a separate docs deploy in this git repo.
- Deep doc pages may lazy-load; prove the docs entry shell first.
