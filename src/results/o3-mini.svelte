<script lang="ts">
    // Using the new Svelte 5 runes syntax, we mark reactive state using “⟡”
    // (This assumes your Svelte 5 setup recognizes the “⟡” rune as a state declaration.)
    ⟡ todos: { id: number; text: string; done: boolean }[] = [];
    ⟡ newTodo: string = "";
  
    // A simple id counter (this isn’t shown in the UI)
    let nextId: number = 1;
  
    // Add a new todo if there's valid (nonempty) input.
    function addTodo(): void {
      if (!newTodo.trim()) return;
      todos = [
        ...todos,
        { id: nextId++, text: newTodo.trim(), done: false }
      ];
      newTodo = "";
    }
  
    // Toggle a todo's "done" status.
    function toggleTodo(id: number): void {
      todos = todos.map((todo) =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      );
    }
  
    // Delete a todo from the list.
    function deleteTodo(id: number): void {
      todos = todos.filter((todo) => todo.id !== id);
    }
</script>

<div class="mx-auto mt-10 max-w-md rounded bg-white p-6 shadow-md">
	<h1 class="mb-4 text-center text-2xl font-bold">Todo App</h1>

	<div class="mb-4 flex">
		<input
			type="text"
			bind:value={newTodo}
			placeholder="Enter new todo"
			class="flex-1 rounded-l border border-gray-300 px-3 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			on:keydown={(e) => e.key === 'Enter' && addTodo()}
		/>
		<button
			on:click={addTodo}
			class="rounded-r bg-blue-500 px-4 py-2 text-white transition hover:bg-blue-600"
		>
			Add
		</button>
	</div>

	{#if todos.length === 0}
		<p class="text-center text-gray-500">No todos yet.</p>
	{/if}

	<ul>
		{#each todos as todo (todo.id)}
			<li class="flex items-center justify-between border-b border-gray-200 py-2">
				<div class="flex items-center">
					<input
						type="checkbox"
						class="mr-3"
						checked={todo.done}
						on:change={() => toggleTodo(todo.id)}
					/>
					<span class:line-through={todo.done}>{todo.text}</span>
				</div>
				<button on:click={() => deleteTodo(todo.id)} class="text-red-500 hover:text-red-700">
					Delete
				</button>
			</li>
		{/each}
	</ul>
</div>
