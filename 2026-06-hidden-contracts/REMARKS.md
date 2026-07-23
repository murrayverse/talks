# Remarks: The Hidden Contracts Behind "The API Broke"

*For readers who may or may not have attended the talk. This is the argument in prose, not a transcript.*

---

## The one-sentence thesis

When AI automation "breaks" a business API, the HTTP layer is usually fine — what failed is an unwritten **hidden contract** that humans used to keep by hand.

---

## Why this matters now

Teams are wiring models and agents into ERPs, CRMs, and ops tools through the same APIs humans used via forms and tribal knowledge. The agent is literal: it optimises for what it can see (field names, status codes). A `200 OK` becomes the goal. **Goodhart's Law** applies: the score stops measuring success.

So the post-mortem line *"the API broke"* is usually a misdiagnosis. The API did what it was specified to do. The **business meaning** of the call was never made enforceable.

---

## Mental model: two contracts

| Contract | What it is |
|---|---|
| **Explicit** | Endpoints, schemas, status codes — what's in the docs |
| **Hidden** | What the experienced colleague just *knows* |

Hidden contracts bite in at least five ways (all compatible with `200 OK`):

1. **Meaning** — a field doesn't mean what it looks like  
2. **Hidden workflow** — one action quietly runs a chain (side effects the UI always ran)  
3. **Background** — who you are changes the answer (company, locale, defaults)  
4. **Sequence** — steps must happen in order  
5. **Retry** — doing it twice must not create two records  

---

## Three example shapes (from the deck)

These are deliberately generic — same patterns show up in many systems.

### 1. Stable keys are not numeric IDs

Humans treat a stable external key (`product.steel_bracket`) and a numeric primary key (`1842`) as "the product." After a renumber/migrate, the number changes (`991`). An agent that hardcoded the number still gets `200 OK` — linked to the wrong row, or to nothing. **Identity is a contract.**

### 2. Deploy before migrating

Ship the new code path first; migrate the data (or identifiers) later. The schema looks fine. Writes succeed. The world the old records live in never entered the new path. **Order of operations is a contract.** Deploy and migrate are not interchangeable.

### 3. XML / external IDs vs numeric IDs

Docs and humans treat `xml_id` and numeric PK as interchangeable labels. Endpoints do not. Swap them and you can still get `200 OK` with a wrong link or a silent no-op. **Meaning contract, not a network failure.**

---

## What to do about it

Better models alone won't save you. Make tribal knowledge **explicit and enforced**.

Practical move from the talk: **don't expose raw create** to the agent. Hand it **one safe verb** — in the deck, `create_confirmed_thing` — that:

1. Runs the real form / business logic (defaults, validations, side effects)  
2. Preferentially supports a dry-run / diff so `200` isn't the only signal  
3. Carries an **idempotency key** so retries don't duplicate  

In four steps: raw create is the trap → hide that door → hand one safe verb → enforce the hidden contract inside it.

---

## Takeaways

1. A `200 OK` is not proof the business result is correct.  
2. Every system has two contracts: documented and tribal.  
3. Failures show up as meaning, sequence, identity, retries — not always as HTTP errors.  
4. Don't give the AI the raw API. Give it one safe verb that keeps the promises.

Closing line from the talk:

> *"The API broke" is a review comment about **your** system, not the model's.*

---

## Who this is for

Developers, automation / integration engineers, and anyone putting agents in front of real systems. Aimed at a mixed audience: plain language first, sharp enough for people who've already been burned.

## Related files

- [Slides (HTML)](./Hidden_Contracts_Slides.html) · [PDF](./Hidden_Contracts_Slides.pdf)  
