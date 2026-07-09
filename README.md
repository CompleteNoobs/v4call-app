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

> Status: **production** — hosted static at `v4call.com`, talking to `node.v4call.com`.

## What's built beyond the monolith carve

- **v0.17 Part A — paid expert invites (desktop client, shipped + live-proven 2026-07-07).** Admin-only 💎
  button in the room Participants header → offer-builder modal (online-user picker, connect fee + rate/hr +
  currency + max duration, live escrow-cap total, Keychain funding). The expert gets an accept-terms modal
  with the exact contract; accepting joins them text-only with a 💎 badge. Both sides get a live session
  ticker (admin = spend, expert = earnings net of platform fee) that freezes the moment the session ends
  ("settling…") and clears on the final receipt.
- **Honest money receipts.** Call receipts show the full breakdown (connect / metered deposit / duration cost /
  refund / platform fee / net), and a loud "⚠ funds still held in escrow" banner whenever a settlement comes
  back pending/failed — plus a follow-up "✓ settlement completed" notice when the escrow box's recovery retry
  lands the payouts.
- **v0.18 — outgoing call quality selector (desktop, shipped 2026-07-09).** Auto / Low / Medium / High dropdown
  that caps the user's *own* outgoing video bitrate + capture resolution (sender-side only — WebRTC gives no way
  to throttle what a peer sends *you*, so each user controls only what they send). A pre-call picker in the
  lobby's ONLINE header and a live in-call picker in the control pill; mid-call changes apply instantly via
  `RTCRtpSender.setParameters()` / `track.applyConstraints()` with no renegotiation. Choice persists in
  `localStorage` (`v4call:quality`). Helps callers on slow/low-data connections.
- **v0.19 — local self-recording (desktop, shipped + tested 2026-07-09).** Red ⏺ Record button in the in-call
  control pill opens an options modal (format WebM/MP4, probed via `MediaRecorder.isTypeSupported` · one file
  vs. separate audio+video · new part every N minutes). Records **only the user's own** outgoing media (their
  `localStream` — mic/cam, or the screen while sharing); never other participants, so there's no signalling and
  no recording-consent problem. Streams each 1 s timeslice straight to a user-picked folder via the File System
  Access API so RAM stays flat; time-splits by stop/restart so every part is a complete standalone file, and
  also cuts a fresh part whenever mic/cam/screenshare is toggled. Each part gets an ISO-8601-UTC filename plus a
  sidecar `.json` (start/end/duration/mediaKind/files) so an editor can line the parts up. Settings persist in
  `localStorage` (`v4call:record`).
  - **Browser support: Chrome or Edge only.** Brave disables the File System Access API by default (privacy
    setting), so recording won't work there — the button shows a "needs Chrome/Edge" message instead of failing
    silently. (Confirmed on Brave 2026-07-09.)
