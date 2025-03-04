<script lang="ts">
	// Define Todo type
	type Todo = {
		id: number;
		text: string;
		completed: boolean;
	};

	// State using runes
	let todos = $state<Todo[]>([]);
	let newTodoText = $state<string>('');
	let nextId = $state<number>(1);

	// Add new todo
	function addTodo() {
		if (newTodoText.trim() === '') return;

		todos.push({
			id: nextId,
			text: newTodoText.trim(),
			completed: false
		});
		newTodoText = '';
		nextId++;
	}

	// Toggle todo completion
	function toggleTodo(id: number) {
		const todo = todos.find((t) => t.id === id);
		if (todo) {
			todo.completed = !todo.completed;
		}
	}

	// Delete todo
	function deleteTodo(id: number) {
		todos = todos.filter((t) => t.id !== id);
	}

	// Handle enter key for adding todo
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			addTodo();
		}
	}
</script>

<div class="mx-auto mt-10 max-w-md rounded-xl bg-white p-6 shadow-md">
	<!-- Header -->
	<h1 class="mb-6 text-2xl font-bold text-gray-800">Todo List</h1>

	<!-- Input Form -->
	<div class="mb-6 flex gap-2">
		<input
			type="text"
			bind:value={newTodoText}
			on:keydown={handleKeydown}
			placeholder="Add a new todo..."
			class="flex-1 rounded-lg border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
		/>
		<button
			on:click={addTodo}
			class="rounded-lg bg-blue-500 px-4 py-2 text-white transition-colors hover:bg-blue-600"
		>
			Add
		</button>
	</div>

	<!-- Todo List -->
	{#if todos.length === 0}
		<p class="text-center text-gray-500">No todos yet. Add one above!</p>
	{:else}
		<ul class="space-y-3">
			{#each todos as todo (todo.id)}
				<li
					class="flex items-center gap-3 rounded-lg bg-gray-50 p-3 transition-colors hover:bg-gray-100"
				>
					<input
						type="checkbox"
						checked={todo.completed}
						on:change={() => toggleTodo(todo.id)}
						class="h-5 w-5 rounded text-blue-500 focus:ring-blue-500"
					/>
					<span class="{todo.completed ? 'text-gray-500 line-through' : 'text-gray-800'} flex-1">
						{todo.text}
					</span>
					<button
						on:click={() => deleteTodo(todo.id)}
						class="p-1 text-red-500 transition-colors hover:text-red-600"
					>
						✕
					</button>
				</li>
			{/each}
		</ul>
	{/if}
</div>
