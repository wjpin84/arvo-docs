# Writing for Arvo's docs

Not published — this is for whoever writes the next page. It exists because a
2026-09-26 review found the documentation accurate and hard to absorb, and the
fixes were all the same fix applied in different places.

## The one rule

**Lead with what the reader gets. Explain the machinery second.**

Arvo's engineering is genuinely unusual, which makes it tempting to open with
the architecture. Resist it — the reader has not yet agreed that the problem is
worth solving.

| Don't open with | Open with |
|---|---|
| "The engine maintains a content-addressed data library." | "Your research stays reproducible: Arvo records the exact data a result came from." |
| "The gate that sized every backtested trade sizes the live ones." | "Your risk controls are the same in research and live trading." |
| "A halt stays halted across a restart." | "If trading is halted, restarting Arvo doesn't quietly remove the halt." |

Then the machinery, then a link to the reference. Three layers, so a reader can
stop at the one they needed:

1. **Promise** — what changes for them.
2. **How it works** — one paragraph, and a diagram if there is a flow.
3. **Detail** — a link, not an expansion.

## Define a term before you lean on it

Never use *deflation*, *out-of-sample*, *regime*, *walk-forward*, *divergence* or
*conservative costs* in a sentence that depends on the reader already knowing it.

Every one of those is in `includes/abbreviations.md`, which is appended to every
page, so it renders as a tooltip automatically. **Add new jargon there in the
same commit that introduces it.** The tooltip is not a substitute for explaining
a concept you are building an argument on — it is for the reader who met the word
three pages ago.

## Audience, per page

Four readers use this site and they should not compete on one page:

| Reader | Asks | Lives in |
|---|---|---|
| Prospective user | "What is this for?" | Home, Start here |
| User | "How do I do the thing?" | Use Arvo, Build strategies |
| Developer | "How do I drive it / extend it?" | Run it headless, Extend, Reference |
| Architect | "Why is it built this way?" | About, and the ADRs |

If a page is answering two of these, split it or link out.

## Prose

- **One idea per paragraph.** Three concepts in one paragraph is the most common
  problem in this repo's history.
- **Verbs, not nominalisations.** "Arvo records every decision", not "the
  provision of an auditable representation".
- **Why before how.** The reason a check exists is more persuasive than the check.
- **Say the cost.** Every page that claims a benefit should be willing to name
  what it costs. This is the house style and it is why the docs are credible.

## Tone

The aphorisms are an asset and a hazard. *"A number always looks like an
answer."* earns its place. Five of those on one page reads as a manifesto.

- **Home, Why Arvo, section openings:** a good line is welcome.
- **How-to, reference, CLI:** flatly literal. No epigrams. The reader is trying
  to do something.

Clear beats clever everywhere it competes.

## Things that are decisions, not oversights

Don't "fix" these without checking:

- **No screenshots yet.** Deferred until the desktop is in a good state; #232 is
  an open bug where a panel does not appear.
- **`Inconclusive` is described as an answer**, never as a failure or a
  limitation.
- **Rule / ruleset rather than strategy** in prose. `strategy` stays in code, the
  protocol and stored findings, and the glossary explains why.
- **Known blind spots are documented**, e.g. deflation cannot see survivorship
  bias. A defence documented without its limits misleads.

## Before you commit

```sh
python -m mkdocs build --strict      # every nav entry and internal link resolves
python -m mkdocs serve               # look at it
```

`--strict` is what CI runs. It catches broken links, not broken mermaid — a
diagram with a syntax error builds fine and fails in the browser, so open the
page.

## Check the claim against the code

The same review found three pages stating things that were true when written and
were no longer:

- the CLI page documented seven verbs of eleven;
- a page said you could not write your own rules, which rules-as-data changed;
- an example used a Python signature that had moved.

**Read the source before documenting behaviour.** A wrong instruction costs the
reader more than an unwritten page.
