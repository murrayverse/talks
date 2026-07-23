# AI Automation vs. Legacy APIs

### The Hidden Contracts Behind "The API Broke"

**Sean Murray** · ~10–12 min · 2026

A field report on why AI automation and business systems fight — and why it is almost never the API's fault.

---

## For people who were (or weren't) in the room

This folder is the public archive of the talk. You do **not** need to have attended to follow it.

**If you attended:** the slides are here to replay the examples; [Remarks](./REMARKS.md) restates the argument in prose if you want the full through-line.

**If you didn't:** start with [Remarks for readers](./REMARKS.md) (thesis + slide-by-slide argument), then open [`Hidden_Contracts_Slides.html`](./Hidden_Contracts_Slides.html) in a browser. The PDF is a static fallback; example panels animate in the HTML.

| Asset | What it is |
|---|---|
| [`Hidden_Contracts_Slides.html`](./Hidden_Contracts_Slides.html) / [`Hidden_Contracts_Slides.pdf`](./Hidden_Contracts_Slides.pdf) | 9-slide deck (HTML preferred) |
| [`REMARKS.md`](./REMARKS.md) | Standalone write-up for non-attendees |

## Abstract

You wire AI automation into a business system through its API. Every call returns `200 OK`. The records look perfect and are quietly wrong. By morning someone says: *"the API broke."*

It didn't. What broke was a **hidden contract** — a rule about meaning, sequence, identity, or retries that nobody wrote down, and that a human operator had been quietly fulfilling. The talk offers a simple model (every system has two contracts), three concrete failure shapes, and one fix: don't hand the AI the raw create endpoint; hand it **one safe verb** that enforces the real contract (illustrated as `create_confirmed_thing`).

## Key lines

- "A 200 OK is the most dangerous thing the AI can hear."
- "All five can return 200 OK while being wrong."
- "'The API broke' is a review comment about your system, not the model's."

## How to view

```bash
open Hidden_Contracts_Slides.html          # preferred (example panels animate)
# or
open Hidden_Contracts_Slides.pdf           # static export
```


## About the examples

Examples are deliberately **vendor-neutral**: no employer or product names. They use generic identifiers and `thing` / `create_confirmed_thing` so the same patterns apply to any business system.
