# Register Learning

My personal, growing record of the programming concepts I learn during my coding sessions — one log file per technology, plus the Claude Code skill that writes into them.

## Why this repo exists

When I code with an AI assistant, it often produces code using a data structure, a language feature, or an API I don't actually know yet. Instead of moving on and forgetting it, I ask the assistant to explain it and log it here. 

Each entry is written as if for a complete beginner: what the concept is, why it matters, how it works, and when to use it — with a small, generic code example.

## How it works

`/register-learning` is a Claude Code skill (defined in [skills/register-learning/SKILL.md](skills/register-learning/SKILL.md)) that takes a topic and a piece of knowledge, then appends a properly formatted entry to the matching log file.

```
/register-learning JS  AbortController lets you cancel an in-flight fetch
```

The skill takes care of the bookkeeping:

- **Resolves the topic to a file** — `JS` → [javascript/JS_Learning.md](javascript/JS_Learning.md), `Lit` → [lit/Lit_Learning.md](lit/Lit_Learning.md), and so on (`{topic-lowercase}/{Topic}_Learning.md`). It creates the folder and file if the topic is new.
- **Reads the whole file first** and checks the new tip against existing entries, so I get asked about merging instead of ending up with duplicates.
- **Generalizes the content** — project names, business logic, and internal identifiers are stripped out and replaced with neutral examples, so entries stay useful (and shareable) outside the codebase they came from.
- **Writes a teaching entry**: explanation, commented code example, a Mermaid diagram or comparison table when the concept involves a flow or trade-off, and a "Further reading" link to authoritative docs.
- **Keeps numbering and the index in sync**, and offers to reorganize the log into chapters once it grows past ~15 entries.

The topic is mandatory — if I don't give one, the skill asks rather than guessing.

## Repository layout

| Path | Contents |
| --- | --- |
| [javascript/JS_Learning.md](javascript/JS_Learning.md) | JavaScript entries |
| [typescript/TypeScript_Learning.md](typescript/TypeScript_Learning.md) | TypeScript entries |
| [lit/Lit_Learning.md](lit/Lit_Learning.md) | Lit / web components entries |
| [courses/](courses/) | Raw notes taken from talks and courses (not skill-generated) |
| [skills/register-learning/](skills/register-learning/) | The skill definition |

Every log file follows the same shape: a short intro, an `## Index` of numbered links, then `## Entries` in chronological order (oldest first).

## Setup

The skill lives in this repo and is exposed to Claude Code through a symlink, so editing `SKILL.md` here takes effect immediately:

```bash
ln -s ~/Projects/learning/skills/register-learning ~/.claude/skills/register-learning
```

The skill derives the log base path from its own resolved location (two directories up from `SKILL.md`), which is why the symlink — rather than a copy — matters.
