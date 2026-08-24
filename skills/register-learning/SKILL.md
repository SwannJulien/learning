---
name: register-learning
description: 'Log a knowledge entry (rule of thumb, best practice, lesson learned) into a topic-specific learning file. Use when: register learning, keep this knowledge, log this in {topic}, add learning tip, register-learning.'
argument-hint: 'Required: the topic file (e.g. JS, Java, PostgreSQL) and the knowledge to log'
---

# Register Learning

## When to Use
- The user has just learned something from an AI conversation (best practice, difference between approaches, gotcha, rule of thumb) and wants to persist it.
- The user explicitly says "keep this knowledge in {topic}" or "register-learning" or "log this in {topic}".

## Topic Resolution
The user provides a topic name (e.g. "JS", "Java", "PostgreSQL", "Lit", "CSS", "Docker"). Map it to a file:

- **Base path**: Run `readlink` (or equivalent) on this skill file's path to resolve the symlink to its real location, then go two directories up. Concretely: `realpath "$(dirname SKILL.md)/../.."`. For example, if the symlink resolves to `/Users/me/Projects/PERS/learning/skills/register-learning/SKILL.md`, the base path is `/Users/me/Projects/PERS/learning/`. **The base path must contain a `skills/` subfolder — if it doesn't, you resolved it wrong. Never write files inside `~/.copilot/`.**
- **Convention**: `{topic-lowercase}/{Topic}_Learning.md`
  - "JS" or "JavaScript" → `javascript/JS_Learning.md`
  - "Java" → `java/Java_Learning.md`
  - "PostgreSQL" or "Postgres" → `postgresql/PostgreSQL_Learning.md`
  - "Lit" → `lit/Lit_Learning.md`
  - etc.

If the subfolder or file doesn't exist yet, create them with this skeleton:

```markdown
# {Topic} Learning Log

A running personal collection of {Topic} rules of thumb, good practices, and lessons learned, gathered day after day while working on real projects (but written generically so they're useful anywhere).

Entries are logged chronologically (oldest first). This log will be reorganized into logical chapters once it grows large enough to need them.

## Index

## Entries
```

The user **must** specify the topic explicitly — if they don't, ask which topic file to use.

## Procedure

### 1. Read the whole document first
Never append blindly — read the full file to know the current entry count, existing titles/topics, and whether it's still in flat/chronological mode or has already been split into chapters.

### 2. Check for duplicates or overlap
Compare the new tip against existing entry titles and content. If a very similar rule already exists:
- **Ask the user** whether to merge/expand the existing entry or add a distinct new one. Do not silently duplicate.

### 3. Generalize the content
The source may come from a specific project/codebase. Strip out project-specific names, business logic, and file paths — rewrite the explanation and code example so they read as generic guidance that would make sense in any project using that technology.

### 4. Compose the entry
Each entry follows this exact shape (no dates, per user preference):

```markdown
### N. <Short, descriptive title>

<Explanation written in a clear, teacher-like tone — as if you're explaining the concept to a colleague who is smart but unfamiliar with this specific topic. Use complete sentences and short paragraphs (2-4 sentences each). Explain *why* something works a certain way, not just *what* it does. When comparing options, explain each one in its own sentence so the reader can follow the reasoning.>

\`\`\`<language>
// Small, self-contained, generic example illustrating the rule (when relevant)
\`\`\`
```

- `N` is the next sequential number (continue numbering across the whole document, even across chapters).
- **Write like a teacher, not a telegram.** Use complete sentences — but mix formats freely to make the content scannable and memorable: short paragraphs for context, bullet points for listing options, tables for comparisons, "Do / Don't" pairs for common mistakes, and well-commented code examples to tie it all together.
- The **commented code example is often the most valuable part** — invest effort in making comments clear and illustrative. A reader should be able to understand the rule just by reading the code + comments.
- Keep it concise but readable — aim for clarity over brevity or verbosity. Use whatever structure makes the concept easiest to grasp at a glance.
- Include a code/config example **only when it adds clarity** — purely conceptual tips can omit it.
- Use the appropriate language tag for the fenced code block (`js`, `java`, `sql`, `html`, `css`, `yaml`, etc.).

### 5. Decide where it goes: flat log vs. chapters

- **Flat/chronological mode** (default until the log grows large): append the new entry at the bottom of `## Entries`, and add a matching link at the bottom of `## Index`.
- **Chapter mode**: if the document has already been split into chapters (`## <Chapter Name>` headings under `## Entries`), file the new entry under the chapter that matches its topic, creating a new chapter heading if none fits. Update the `## Index` accordingly (grouped by chapter).

### 6. When to propose reorganizing into chapters
Once the flat list reaches roughly **15 entries**, or once **3+ entries** clearly cluster around the same topic, **ask the user** before restructuring:

> "The log has grown — want me to reorganize it into chapters? I'll group existing entries by topic and keep the numbering intact."

Only restructure after explicit confirmation.

### 7. Confirm
After editing, briefly tell the user which entry number/title was added, in which file, and where (flat list or which chapter).
