# JavaScript Learning Log

A running personal collection of JavaScript rules of thumb, good practices, and lessons learned, gathered day after day while working on real projects (but written generically so they're useful anywhere).

Entries are logged chronologically (oldest first). This log will be reorganized into logical chapters once it grows large enough to need them — see the `js-learning` skill for how new entries are added.

## Index

1. [Empty string vs `null` for initial values](#1-empty-string-vs-null-for-initial-values)

## Entries

### 1. Empty string vs `null` for initial values

When initializing a property/variable, the choice between `''` and `null` should be driven by how the value is used later, not by habit:

- Use **`''`** when the value is always treated as a string — you'll call string methods on it (`.trim()`, `.length`, `.slice()`, etc.) or bind it directly into a template/input that expects a string. This avoids `TypeError: Cannot read properties of null` crashes.
- Use **`null`** when the value is an optional reference or ID that may not exist yet — especially if it will be serialized into a JSON payload sent to an API. `null` explicitly communicates "no value", whereas `''` could be misread by the receiving side as a real but blank value.
- Mixing the two up is a common source of bugs: calling `.trim()` on a `null` throws; sending `''` where an API expects "absent" can silently break "is this new?" logic on the backend.

```js
class ChatState {
  constructor() {
    // Always used with string methods / template bindings -> safe string default
    this.draftText = '';

    // Optional external reference, not created yet -> explicit "no value"
    this.threadId = null;
  }

  submit() {
    const trimmed = this.draftText.trim(); // safe: draftText is always a string

    return {
      thread_id: this.threadId, // null tells the API "start a new thread"
      message: trimmed,
    };
  }
}
```

**Further reading:** [MDN Web Docs: null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)
