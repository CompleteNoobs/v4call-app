# v4call-app

The **standalone v4call client** — all GUIs in one bundle. Desktop + mobile layout files plus every operator/util
page (admin-peers, rate-editor, server-sign / server-announce, user-announce, key generators). Connects to a
chosen **v4call-node** via `nodeBase()`; can be hosted as a web app or packaged later.

- **Version:** 0.1.0
- Succeeds the frontend of the monolith **v4call** (final version **v0.16.29**), carved out per the decoupling plan.
- Talks to **v4call-node** over Socket.io + HTTP. Shares envelope/crypto with the node via a vendored
  plain-JS `shared/`.
- **Source of truth:** [`../handover-decoupling.md`](../handover-decoupling.md)

> Status: scaffold. Client extracted during the decoupling build (see the hand-off doc, build sequence §11).
> First step is the behaviour-neutral `nodeBase()` hygiene on the monolith's `app.html` (hand-off doc §B).
