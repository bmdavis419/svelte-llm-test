<script lang="ts">
	// Define Todo type
	type Todo = {
		id: number;
		text: string;
		completed: boolean;
	};

	// Reactive state for todos array
	let todos = $state<Todo[]>([
		{ id: 1, text: 'Learn Svelte 5', completed: false },
		{ id: 2, text: 'Build a todo app', completed: false }
	]);

	// Reactive state for new todo input
	let newTodoText = $state('');

	// Derived value for todo count
	let remainingTodos: number = $derived(todos.filter((todo) => !todo.completed).length);

	// Functions for todo operations
	function addTodo() {
		if (!newTodoText.trim()) return;
		todos.push({
			id: Date.now(),
			text: newTodoText,
			completed: false
		});
		newTodoText = '';
	}

	function toggleTodo(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	}

	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}
</script>

<div class="mx-auto mt-8 max-w-md rounded-lg bg-white p-6 shadow-md">
	<h1 class="mb-6 text-2xl font-bold text-gray-800">Todo List</h1>

	<!-- Add Todo Form -->
	<form
		class="mb-6 flex gap-2"
		onsubmit={(e) => {
			e.preventDefault();
			addTodo();
		}}
	>
		<input
			type="text"
			bind:value={newTodoText}
			placeholder="Add a new todo..."
			class="flex-1 rounded border p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
		/>
		<button
			type="submit"
			class="rounded bg-blue-500 px-4 py-2 text-white transition-colors hover:bg-blue-600"
		>
			Add
		</button>
	</form>

	<!-- Todo List -->
	<ul class="space-y-2">
		{#each todos as todo}
			<li class="flex items-center rounded bg-gray-50 p-3 transition-colors hover:bg-gray-100">
				<input
					type="checkbox"
					checked={todo.completed}
					onclick={() => toggleTodo(todo.id)}
					class="mr-3 h-5 w-5 rounded text-blue-500"
				/>
				<span
					class:text-gray-400={todo.completed}
					class:line-through={todo.completed}
					class="flex-1"
				>
					{todo.text}
				</span>
				<button
					onclick={() => deleteTodo(todo.id)}
					class="ml-2 p-1 text-red-500 transition-colors hover:text-red-700"
				>
					×
				</button>
			</li>
		{/each}
	</ul>

	<!-- Todo Count -->
	<p class="mt-4 text-sm text-gray-600">
		{remainingTodos}
		{remainingTodos === 1 ? 'task' : 'tasks'} remaining
	</p>
</div>
