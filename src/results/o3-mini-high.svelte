<script lang="ts">
	// Define a Todo interface
	interface Todo {
		id: number;
		text: string;
		done: boolean;
	}

	// Reactive state for todos and new todo input
	let todos = $state<Todo[]>([
		{ id: 1, text: 'Learn Svelte 5', done: false },
		{ id: 2, text: 'Build a todo app', done: false }
	]);
	let newTodo = $state<string>('');
	let nextId = 3;

	// Derived value for count of remaining (not done) todos
	let remaining = $derived(todos.filter((todo) => !todo.done).length);

	// Add a new todo
	function addTodo() {
		if (newTodo.trim()) {
			todos.push({ id: nextId++, text: newTodo.trim(), done: false });
			newTodo = '';
		}
	}

	// Mark a todo as done
	function markDone(id: number) {
		const index = todos.findIndex((todo) => todo.id === id);
		if (index !== -1) {
			todos[index].done = true;
		}
	}

	// Delete a todo
	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}
</script>

<div class="mx-auto mt-10 max-w-md rounded border border-gray-200 bg-white p-6 shadow">
	<h1 class="mb-4 text-center text-2xl font-bold">Todo App</h1>

	<!-- New Todo Input -->
	<div class="mb-4">
		<input
			type="text"
			placeholder="Enter new todo"
			class="w-full rounded border px-3 py-2 focus:border-blue-300 focus:ring focus:outline-none"
			bind:value={newTodo}
		/>
		<button
			class="mt-2 w-full rounded bg-blue-500 px-3 py-2 text-white hover:bg-blue-600"
			onclick={addTodo}
		>
			Add Todo
		</button>
	</div>

	<!-- Remaining todos count -->
	<p class="mb-2 text-gray-700">Remaining: {remaining}</p>

	<!-- Todo List -->
	<ul>
		{#each todos as todo (todo.id)}
			<li class="flex items-center justify-between border-b p-2 last:border-b-0">
				<span class={todo.done ? 'text-gray-500 line-through' : 'text-gray-800'}>
					{todo.text}
				</span>
				<div class="flex space-x-2">
					{#if !todo.done}
						<button
							class="rounded bg-green-500 px-2 py-1 text-white hover:bg-green-600"
							onclick={() => markDone(todo.id)}
						>
							Done
						</button>
					{/if}
					<button
						class="rounded bg-red-500 px-2 py-1 text-white hover:bg-red-600"
						onclick={() => deleteTodo(todo.id)}
					>
						Delete
					</button>
				</div>
			</li>
		{/each}
	</ul>
</div>
