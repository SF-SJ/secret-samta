# Sealed Hat

A Secret Santa draw where **only the giver ever sees their name** — including the person
organising it.

One file, no build step, no server, no accounts, no database. Open `index.html` and it works.

## The rules it keeps

- Nobody draws themselves.
- Couples (or housemates, or anyone who already buys for each other) never draw each other.
  Rules block the pairing in both directions.
- Everyone ends up in a single gift-giving loop, so the group never splits into two cliques
  quietly buying among themselves.
- The organiser never sees an assignment list. There isn't one: the draw exists only in the
  page's memory, and each name leaves the page only inside that person's own link.

## Two ways to hand the names out

**Everyone's in the room.** Pass the phone round. Each person taps their own name, reads it,
taps *Done*. A name that has been read can't be reopened — by them or by you — and once the
last person has read theirs the draw is wiped from memory. Nothing to trust; nobody, including
the organiser, can see anyone else's.

**They're somewhere else.** The page produces one link per person and never shows you what's
inside any of them. Copy each message into WhatsApp, Signal, email, whatever. The recipient's
name is scrambled inside the link's `#fragment` with a random key, so the link is unreadable
as text and is never sent to any server — the fragment never leaves the browser that opens it.

The honest caveat for the second mode: whoever holds a link can open it. The page is built so
you never see a name by accident, but it can't stop a determined organiser from clicking. If
you want that guaranteed, use the pass-the-phone mode, or use a service that emails people
directly.

## Running it

Open `index.html` in a browser. That's the whole thing.

For the link mode the page needs a web address, so that other people's browsers can load it:

- **GitHub Pages** — Settings → Pages → deploy from this branch, root. The page lands at
  `https://<user>.github.io/secret-samta/`.
- Or drop `index.html` on any static host.

Opened as a local `file://` the app still runs and says so: pass-the-phone works, but links
generated there only work on that same computer.

## How the draw works

`drawNames()` shuffles the participants into a random order and walks the cycle — each person
gives to the next, the last gives to the first. A cycle guarantees no self-assignment and one
connected loop for free. If the order violates an exclusion rule it's thrown away and reshuffled
(up to 40,000 times), which samples uniformly from the valid cycles. Shuffling uses
`crypto.getRandomValues` with rejection sampling, so there's no modulo bias.

Rule sets with no possible draw — two people who can't draw each other, one person excluded from
everyone — are detected and reported instead of hanging.
