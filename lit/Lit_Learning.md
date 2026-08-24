# Lit Learning Log

A running personal collection of Lit rules of thumb, good practices, and lessons learned, gathered day after day while working on real projects (but written generically so they're useful anywhere).

Entries are logged chronologically (oldest first). This log will be reorganized into logical chapters once it grows large enough to need them.

## Index

- [1. Public vs private vs constructor-only properties](#1-public-vs-private-vs-constructor-only-properties)

## Entries

### 1. Public vs private vs constructor-only properties

Lit gives you three levels of property declaration, each with different visibility and reactivity characteristics. Choosing the right one avoids unnecessary re-renders and keeps your component's API clean.

**Public reactive properties** (`@property()` or `{ type: String }` in `static properties`) are the component's public API. They can be set via HTML attributes or JavaScript, and any change triggers a re-render. Use them for data that consumers of your component are expected to pass in.

**Private reactive properties** (`@state()` or `{ state: true }`) are internal to the component but still trigger a re-render when they change. Use `state: true` when the property holds internal state that the template depends on — for example, a loading flag, a toggle state, or fetched data that the template renders. Because `state: true` suppresses attribute handling, these properties cannot be set from the outside via HTML.

**Constructor-only variables** (plain class fields or assignments in `constructor()`) are not declared in `static properties` at all. They do **not** trigger re-renders when mutated. Use them for bookkeeping data that the template never reads — timeout IDs, cached DOM references, AbortControllers, internal counters for logic, or configuration that never changes after initialization.

| Scenario | Declaration | Triggers render? | Settable from outside? |
|----------|-------------|-----------------|----------------------|
| Consumer passes data in | `@property()` | Yes | Yes (attribute + JS) |
| Internal state shown in template | `@state()` / `state: true` | Yes | No |
| Internal bookkeeping, not rendered | Constructor variable | No | No |

**Should every private property use `state: true`?** No. Only use it when changing that value must cause the component to update its DOM. If the variable is just housekeeping (a timer ID, a reference to a child element, a request controller), declaring it reactive would cause pointless re-renders every time you reassign it.

```js
import { LitElement, html } from 'lit';
import { property, state } from 'lit/decorators.js';

class UserCard extends LitElement {
  // Public: consumers set this via <user-card .userId=${id}>
  @property({ type: String }) userId = '';

  // Private reactive: internal, but template reads it → needs re-render
  @state() _userData = null;
  @state() _loading = false;

  // Constructor-only: bookkeeping, template never shows this
  _abortController = null;

  async connectedCallback() {
    super.connectedCallback();
    this._abortController = new AbortController();
    this._loading = true; // triggers render → shows spinner
    this._userData = await fetchUser(this.userId, this._abortController.signal);
    this._loading = false; // triggers render → shows data
  }

  disconnectedCallback() {
    super.disconnectedCallback();
    this._abortController?.abort(); // no render needed
  }

  render() {
    if (this._loading) return html`<spinner-el></spinner-el>`;
    return html`<p>${this._userData?.name}</p>`;
  }
}
```
