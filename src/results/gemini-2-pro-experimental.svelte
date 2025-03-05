<script lang="ts">
	interface Todo {
		id: number;
		text: string;
		done: boolean;
	}

	let todos: Todo[] = $state([
		{ id: 1, text: 'Learn Svelte 5', done: false },
		{ id: 2, text: 'Build a todo app', done: true },
		{ id: 3, text: 'Profit?', done: false }
	]);
	let nextId = $derived(Math.max(...todos.map((t) => t.id), 0) + 1);
	let newTodoText = $state('');

	function addTodo() {
		if (newTodoText.trim()) {
			todos = [...todos, { id: nextId, text: newTodoText.trim(), done: false }];
			newTodoText = '';
		}
	}

	function toggleDone(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, done: !todo.done } : todo));
	}

	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}

	function handleKeyDown(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			addTodo();
		}
	}
</script>

<div class="container mx-auto max-w-md p-4">
	<h1 class="mb-4 text-center text-2xl font-bold">Svelte 5 Todo App</h1>

	<div class="mb-4">
		<input
			type="text"
			class="w-full rounded border p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			placeholder="Add a new todo..."
			bind:value={newTodoText}
			onkeydown={handleKeyDown}
		/>
		<button
			class="mt-2 w-full rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
			onclick={addTodo}
		>
			Add Todo
		</button>
	</div>

	<ul>
		{#each todos as todo (todo.id)}
			<li class="flex items-center justify-between border-b py-2 last:border-none">
				<div class="flex items-center">
					<input
						type="checkbox"
						class="mr-2"
						checked={todo.done}
						onchange={() => toggleDone(todo.id)}
					/>
					<span class={todo.done ? 'text-gray-500 line-through' : ''}>
						{todo.text}
					</span>
				</div>
				<button
					class="rounded bg-red-500 px-2 py-1 text-xs font-bold text-white hover:bg-red-700"
					onclick={() => deleteTodo(todo.id)}
				>
					Delete
				</button>
			</li>
		{/each}
	</ul>

	{#if todos.length === 0}
		<p class="mt-4 text-center text-gray-500">No todos yet. Add some!</p>
	{/if}
</div>
