# Pricing and Enterprise

Pricing explains self-host versus scaled commercial plans. Enterprise describes identity preview (SSO/SCIM) for commercial deployments.

## Sub-features

- `pricing-load` opens `/pricing` with the self-host headline.
- `enterprise-load` opens `/enterprise` with the enterprise identity preview headline.
- `commercial-nav` reaches both pages from primary or footer navigation.

## How to get to it (user POV)

- Choose `Pricing` in the header.
- Open `https://openconnector.dev/pricing`.
- Open `https://openconnector.dev/enterprise` from Developers / footer Enterprise.

## Driving it with verify-openconnector

Preconditions:

- `verify-openconnector doctor` exited `0`.

- **Pricing.** Run `verify-openconnector fetch /pricing`. HTTP `200`. Body contains `Self-host for free` (or the current H1) and `Open Connector`.
- **Enterprise.** Run `verify-openconnector fetch /enterprise`. HTTP `200`. Body contains `Enterprise identity is in preview` (or the current H1).
- **Screenshots.** Capture `/pricing` and `/enterprise` under `$OPENCONNECTOR_VERIFY_EVIDENCE/features/pricing-enterprise/`.
- **Proof.** Record both titles/H1s in `proof.txt`. No purchase or sales-form submission is required for the default map.

## Gotchas

- Copy on commercial pages changes; if an H1 drifts, update this feature file rather than weakening the journey to a generic 200 OK.
- Do not invent checkout or billing flows that are not linked from these pages.
