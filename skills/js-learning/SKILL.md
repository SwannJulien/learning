---
name: js-learning
description: 'Add a new JavaScript rule of thumb, tip, or lesson learned to the personal JS_Learning.md log in ~/Documents, keeping format and organization consistent across entries. Use when: add JS learning tip, log this JS rule, add javascript note, js-learning, javascript best practice log.'
argument-hint: 'Optional: the rule/tip/observation to log, and the code snippet it came from'
---

# JS Learning Log

## When to Use
- The user shares a JavaScript rule of thumb, gotcha, or best practice they want to remember.
- The user explicitly asks to log/add a JS learning tip, rule, or note.

## File Location
The log lives at `/Users/t026016/Projects/PERS/learning/javascript/JS_Learning.md`. If it doesn't exist yet, create it with this skeleton:

```markdown
# JavaScript Learning Log

A running personal collection of JavaScript rules of thumb, good practices, and lessons learned, gathered day after day while working on real projects (but written generically so they're useful anywhere).

Entries are logged chronologically (oldest first). This log will be reorganized into logical chapters once it grows large enough to need them — see the `js-learning` skill for how new entries are added.

## Index

## Entries
```

## Procedure

### 1. Read the whole document first
Never append blindly — read the full file to know the current entry count, existing titles/topics, and whether it's still in flat/chronological mode or has already been split into chapters.

### 2. Check for duplicates or overlap
Compare the new tip against existing entry titles and content. If a very similar rule already exists:
- **Ask the user** whether to merge/expand the existing entry or add a distinct new one. Do not silently duplicate.

### 3. Generalize the content
The source may come from a specific project/codebase. Strip out project-specific names, business logic, and file paths — rewrite the explanation and code example so they read as generic JavaScript guidance that would make sense in any project.

### 4. Compose the entry
Each entry follows this exact shape (no dates, per user preference):

```markdown
### N. <Short, descriptive title>

<Explanation as 2-4 concise bullet points or a short paragraph. State the rule, when to use each option, and the concrete consequence of getting it wrong.>

\`\`\`js
// Small, self-contained, generic example illustrating the rule
\`\`\`
```

- `N` is the next sequential number (continue numbering across the whole document, even across chapters).
- Keep explanations short — bullet points over prose, no multi-paragraph essays.
- Keep the code example minimal and runnable in isolation (no framework/project-specific imports unless the tip is genuinely framework-specific, in which case name the framework explicitly in the title).

### 5. Decide where it goes: flat log vs. chapters

- **Flat/chronological mode** (default until the log grows large): append the new entry at the bottom of `## Entries`, and add a matching link at the bottom of `## Index`.
- **Chapter mode**: if the document has already been split into chapters (`## <Chapter Name>` headings under `## Entries`), file the new entry under the chapter that matches its topic, creating a new chapter heading if none fits. Update the `## Index` accordingly (grouped by chapter).

### 6. When to propose reorganizing into chapters
Once the flat list reaches roughly **15 entries**, or once **3+ entries** clearly cluster around the same topic (e.g. variables/types, functions/closures, async/promises, objects/arrays, errors, performance, patterns, tooling), **ask the user** before restructuring:

> "The log has grown — want me to reorganize it into chapters (e.g. Variables & Types, Async & Promises, ...)? I'll group existing entries by topic and keep the numbering intact."

Only restructure after explicit confirmation. When restructuring:
- Group entries under `##` chapter headings inside `## Entries`.
- Rebuild `## Index` as a nested list: chapter name, then its entries as sub-links.
- Preserve each entry's original number — do not renumber existing entries.

### 7. Confirm
After editing, briefly tell the user which entry number/title was added and where (flat list or which chapter).
