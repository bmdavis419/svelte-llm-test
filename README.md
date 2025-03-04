# Svelte LLM Test

_a simple test to see how up to date and useful the models are for dealing with svelte 5_

**PROMPT**

> Make a simple client side todo app using svelte 5. Do everything client side, put everything in one component, use typescript, and make sure you use the new runes syntax for state. The user should be able to view their todos, make one as done, create a new done, and delete an old one. Style the component with tailwind, make it clean, pleasant to look at, and simple.

## Models tested

- OpenAI o3-mini
- Claude 3.5 Sonnet
- Gemini 2.0 Flash
- Claude 3.7 Sonnet
- Grok 3

## Results

_all results are in the `src/results` folder_

**TL;DR:** Grok 3 did the best, o3-mini outputted complete nonsense, the rest did ok

### OpenAI o3-mini: 0/10

- failed HARD
- thought that state needed to be prefixed with "⟡"
- still had the old `on:` syntax
- just go look at the code, it's a work of art

### Claude 3.5 Sonnet: 8/10

- got pretty close
- added an extra: `import { let } from 'svelte/runes';` at the top for no reason
- runes syntax was correct
- still had the old `on:` syntax

### Gemini 2.0 Flash: 7/10

- got pretty close
- added an extra: `import { createEventDispatcher } from 'svelte';` import for no reason
- runes syntax was basically correct, but it made the mistake of marking state vars as `const`
- still had the old `on:` syntax

### Claude 3.7 Sonnet: 8/10

- was spot on except for two things:
- randomly imported: `import { $state, $derived } from 'svelte';` (this does nothing and breaks the code)
- still had the old `on:` syntax

### Grok 3: 9.5/10

- got everything right except for still having the old `on:` syntax
