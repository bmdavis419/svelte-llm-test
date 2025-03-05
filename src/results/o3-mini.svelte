<script lang="ts">
	type Todo = { id: number; text: string; done: boolean };

	// Reactive state for todos and new todo input
	let todos = $state<Todo[]>([{ id: Date.now(), text: 'Learn Svelte 5', done: false }]);
	let newTodoText = $state<string>('');

	// Derived state for remaining todos
	let remaining = $derived<number>(() => todos.filter((todo) => !todo.done).length);

	// Add a new todo
	function addTodo() {
		if (!newTodoText.trim()) return;
		todos.push({ id: Date.now(), text: newTodoText.trim(), done: false });
		newTodoText = '';
	}

	// Toggle todo done status
	function toggleDone(todo: Todo) {
		todo.done = !todo.done;
	}

	// Delete a todo
	function deleteTodo(todo: Todo) {
		todos = todos.filter((t) => t.id !== todo.id);
	}
</script>

<div class="mx-auto mt-10 max-w-md rounded border bg-white p-5 shadow">
	<h1 class="mb-4 text-center text-2xl font-bold">Todo App</h1>

	<div class="mb-4">
		<input
			type="text"
			placeholder="Enter new todo"
			bind:value={newTodoText}
			class="w-full rounded border p-2 focus:ring focus:outline-none"
		/>
		<button
			onclick={addTodo}
			class="mt-2 w-full rounded bg-blue-500 p-2 text-white transition hover:bg-blue-600"
		>
			Add Todo
		</button>
	</div>

	<ul>
		{#each todos as todo (todo.id)}
			<li class="flex items-center justify-between border-b p-2">
				<span class={todo.done ? 'text-gray-500 line-through' : ''}>
					{todo.text}
				</span>
				<div>
					<button
						onclick={() => toggleDone(todo)}
						class="mr-2 rounded border p-1 text-sm transition hover:bg-gray-200"
					>
						{todo.done ? 'Undone' : 'Done'}
					</button>
					<button
						onclick={() => deleteTodo(todo)}
						class="rounded border p-1 text-sm transition hover:bg-red-200"
					>
						Delete
					</button>
				</div>
			</li>
		{/each}
	</ul>

	<div class="mt-4 text-center">
		<p>Remaining todos: {remaining}</p>
	</div>
</div>
