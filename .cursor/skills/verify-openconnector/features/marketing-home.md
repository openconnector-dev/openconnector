# Marketing home and waitlist

The marketing home tells a visitor what Open Connector is, shows the brand at hero scale, and offers Join waitlist as the primary call to action without requiring a console account.

## Sub-features

- `home-load` loads `/` with Open Connector branding and the headline Your agents. / Connected.
- `home-nav` exposes Product, Integrations, Skills, Pricing, Docs in primary navigation.
- `waitlist-open` focuses the waitlist email field from the header CTA or hero form.
- `waitlist-validate` rejects an empty or malformed email without claiming a successful signup.

## How to get to it (user POV)

- Open `https://openconnector.dev/`.
- Choose `Join waitlist` in the header (targets `/#waitlist`).
- Use the hero form labeled `Email address` (`id=waitlist-email-hero`) and submit `Join waitlist`.

## Driving it with verify-openconnector

Preconditions:

- `verify-openconnector doctor` exited `0`.
- Evidence directory exists for this run.

- **Load home.** Fetch the marketing root. Run `verify-openconnector fetch /`. HTTP `200` and the body contains `Open Connector` and `Your agents`.
- **Confirm nav.** From the saved body, assert links `/integrations`, `/skills`, `/pricing`, `/docs` and `aria-label="Primary navigation"`.
- **Screenshot home.** Run `verify-openconnector screenshot / "$OPENCONNECTOR_VERIFY_EVIDENCE/features/marketing-home/home.png"`. The image shows the Open Connector wordmark and hero headline.
- **Open waitlist.** In a disposable browser profile, choose the header link `Join waitlist` or focus `#waitlist-email-hero`. The email textbox is visible.
- **Validate empty submit.** Submit the form with an empty email. Observe client validation (field remains / invalid state). Do **not** treat this as a successful waitlist join.
- **Proof.** Save `proof.txt` listing HTTP markers and screenshot path. Do not assert a waitlist confirmation unless the run owns the submitted mailbox and captures the success UI text.

## Gotchas

- `docs.openconnector.dev` redirects to `https://openconnector.dev/docs`; prefer the canonical marketing origin in assertions.
- Submitting a real email is a production side effect. Default proofs stop at validation.
- The hero 3D canvas is decorative; brand + headline + waitlist form are the proof targets.
- SSR HTML may contain null bytes in streamed payloads; use `grep -a` or strip `\0` before parsing.
