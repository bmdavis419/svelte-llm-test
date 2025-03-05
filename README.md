# Svelte LLM Test (with minimal context)

_a simple test to see how up to date and useful the models are for dealing with svelte 5 if given minimal context on the changes_

**PROMPT**

```md
<important_info>
Use Svelte 5's new syntax with TypeScript for reactivity, props, events, and content passing. Prioritize this over Svelte 4 syntax unless specified otherwise.

**Key Changes:**

- **Reactivity:** `$state` for reactive state, `$derived` for computed values, `$effect` for side effects.
- **Props:** Use `$props()` instead of `export let`.
- **Events:** Use HTML attributes (e.g., `onclick`) instead of `on:`.
- **Content:** Use `{#snippet}` and `{@render}` instead of slots.

**Quick Examples:**

1. **State & Events:** `<script lang="ts">let count = $state(0); </script> <button onclick={() => count += 1}>{count}</button>`
2. **Derived:** `let doubled = $derived(count * 2);`
3. **Props:** `<script lang="ts">let { name = 'World' } = $props(); </script> <p>Hello, {name}!</p>`
4. **Binding:** `<script lang="ts">let { value = $bindable() } = $props(); </script> <input bind:value={value} />`
5. **Snippets:** `<div>{@render header()}</div>` with `<Child>{#snippet header()}<h1>Header</h1>{/snippet}</Child>`
6. **Class Store:** `class Counter { count = $state(0); increment() { this.count += 1; } } export const counter = new Counter();`

**Notes:**

- Type `$derived` explicitly (e.g., `let items: Item[] = $derived(...)`) for arrays in TypeScript.
- Default to new syntax for Svelte 5 benefits.
- Avoid stores unless necessary for pub/sub.
  </important_info>

Taking the important_info into account, make a simple client side todo app using svelte 5. Do everything client side, put everything in one component, use typescript, and make sure you use the new runes syntax for state. The user should be able to view their todos, make one as done, create a new done, and delete an old one. Style the component with tailwind, make it clean, pleasant to look at, and simple.
```

## Models tested

- Claude 3.5 Sonnet
- Claude 3.7 Sonnet
- Claude 3.7 Sonnet Extended
- Gemini 2.0 Flash
- Gemini 2.0 Flash Thinking Experimental
- Gemini 2.0 Pro Experimental
- Grok 3
- Grok 3 (Think)
- o3-mini-high
- o3-mini

## Results

_all results are in the `src/results` folder_

All the models did pretty well with instruction compared to the non instruction prompt. `gemini-2-flash` still tried using the `on:` syntax, and `o3-mini` returned a getter from a derived instead of the value (perhaps it meant `$derived.by`).
