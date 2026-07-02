# v4call-app

The **standalone v4call client** — all GUIs in one bundle. Desktop + mobile layout files plus every operator/util
page (admin-peers, server-sign / server-announce, user-announce, key generators). Connects to a
chosen **v4call-node** via `nodeBase()`; can be hosted as a web app or packaged later.

- **`user-announce.html`** is the current rates tool — it replaced the monolith's `rate-editor.html`, which is
  outdated and not carried into this repo. Every in-app link that used to point at `rate-editor.html` now
  points at `user-announce.html`.

- **Version:** 0.1.0
- Succeeds the frontend of the monolith **v4call** (final version **v0.16.29**), carved out per the decoupling plan.
- Talks to **v4call-node** over Socket.io + HTTP. Shares envelope/crypto with the node via a vendored
  plain-JS `shared/`.
- **Source of truth:** [`../handover-decoupling.md`](../handover-decoupling.md)
- **`index.html`** — the landing page (ported from the monolith's marketing page), CTA → `desktop-app.html`,
  plus a `#pages` sitemap linking every page in this bundle. Screenshots live in `v4call-web-pics/`.
- **`walkthrough.wiki`** — deploy guide for hosting this repo as a static site on Alpine Linux, including the
  CORS setup needed when the client and its node are on different domains (the normal case).

> Status: scaffold. Client extracted during the decoupling build (see the hand-off doc, build sequence §11).
> First step is the behaviour-neutral `nodeBase()` hygiene on the monolith's `app.html` (hand-off doc §B).
