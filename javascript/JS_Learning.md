# JavaScript Learning Log

A running personal collection of JavaScript rules of thumb, good practices, and lessons learned, gathered day after day while working on real projects (but written generically so they're useful anywhere).

Entries are logged chronologically (oldest first). This log will be reorganized into logical chapters once it grows large enough to need them — see the `js-learning` skill for how new entries are added.

## Index

1. [Empty string vs `null` for initial values](#1-empty-string-vs-null-for-initial-values)
2. [Dynamic object properties with bracket notation](#2-dynamic-object-properties-with-bracket-notation)
3. [Truthy and falsy values](#3-truthy-and-falsy-values)

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

### 2. Dynamic object properties with bracket notation

Bracket notation, `object[expression]`, evaluates the expression inside the brackets and uses its result as the property key. This makes it useful for lookup tables when the property to read comes from a variable; `itemsByType[type]` is equivalent to `itemsByType['client']` when `type` contains `'client'`.

Dot notation such as `itemsByType.type` searches for a literal property named `type`, so it is not interchangeable with bracket notation. A fallback such as `|| []` can provide a safe default when the computed key does not exist in the object.

```js
const fruits = {
  apple: 'winter fruit',
  banana: 'summer fruit'
};

const selectedFruit = 'apple';

fruits[selectedFruit]; // 'winter fruit'
fruits.apple; // 'winter fruit'
fruits[selectedFruit] || 'unknown fruit'; // Safe fallback if the key is missing
```

**Further reading:** [MDN Web Docs: Property accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors)

### 3. Truthy and falsy values

JavaScript evaluates every value as either truthy or falsy when it is used in a boolean context, such as an `if` condition or with the logical NOT operator (`!`). The complete set of falsy values is `false`, `0`, `-0`, `0n`, `NaN`, `''`, `null`, and `undefined`; all other ordinary JavaScript values are truthy, including empty arrays (`[]`) and empty objects (`{}`).

The `!` operator first converts a value to a boolean and then reverses it. Therefore, `!value` is `true` for any falsy value and `false` for any truthy value. For example, `!country` is `true` when `country` is `undefined` or an empty string, so it can disable a button until a non-empty country value is selected.

```js
const falsyValues = [false, 0, -0, 0n, NaN, '', null, undefined];

falsyValues.forEach(value => {
  Boolean(value); // false
  !value;         // true
});

Boolean('Spain'); // true: every non-empty string is truthy
Boolean([]);      // true: an empty array is still an object
Boolean({});      // true: an empty object is still an object

let country;
!country; // true because country is undefined

country = '';
!country; // true because country is an empty string

country = 'Spain';
!country; // false because country is a non-empty string
```

**Further reading:** [MDN Web Docs: Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) and [MDN Web Docs: Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)
