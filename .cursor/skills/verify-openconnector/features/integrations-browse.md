# Integrations catalog

Integrations lets a visitor browse the public toolkit catalog, search by name, and open a provider detail page that describes tools and auth for AI agents.

## Sub-features

- `catalog-load` opens `/integrations` with the toolkit directory and search field.
- `catalog-search` filters toolkits via `Search toolkits`.
- `catalog-open` opens a provider detail route such as `/integrations/github`.
- `catalog-detail` shows GitHub-for-agents copy and supported tools on the detail page.

## How to get to it (user POV)

- Choose `Integrations` in primary navigation.
- Open `https://openconnector.dev/integrations` directly.
- From the home integrations section, choose Explore all integrations or a provider tile.
- Choose Explore integration on a toolkit card, or open `/integrations/<slug>`.

## Driving it with verify-openconnector

Preconditions:

- `verify-openconnector doctor` exited `0`.
- Network can reach `https://openconnector.dev`.

- **One-shot helper.** Run `verify-openconnector drive-integrations`. Exit code `0`. Evidence under `$OPENCONNECTOR_VERIFY_EVIDENCE/features/integrations-browse/` includes `integrations.body.html`, `github.body.html`, `proof.txt`, and screenshots when Chrome is available.
- **Load catalog.** Run `verify-openconnector fetch /integrations`. HTTP `200`. Body contains `Find your next integration`, `aria-label="Search toolkits"`, and `aria-label="Toolkits"`.
- **Open GitHub detail.** Run `verify-openconnector fetch /integrations/github`. HTTP `200`. Visible text (HTML comments stripped) contains `GitHub`, `for AI agents`, and `Supported GitHub tools and triggers`.
- **Interactive search (browser).** In the disposable profile, open `/integrations`, fill the searchbox named `Search toolkits` with `GitHub`, then open the GitHub result / `/integrations/github`. The detail heading identifies GitHub for AI agents.
- **Proof.** Keep `proof.txt` plus `integrations.png` and `github.png` when screenshots succeed. Both pages must show Open Connector branding.

## Gotchas

- The directory is large (1000+ toolkits). Prefer search or a known slug over scrolling page 57.
- First-page SSR cards may start at `1password`, not GitHub — searching or deep-linking is the stable path.
- Detail pages can be multi-megabyte HTML; assertions should use markers, not full-file diffs.
- Marketing SSR often inserts `<!-- -->` between words; assert on visible text, not raw substrings.
- Do not confuse `View source on GitHub` (repo link) with the GitHub integration card.
