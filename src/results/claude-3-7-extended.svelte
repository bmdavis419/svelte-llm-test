<script lang="ts">
	// Define the Todo type
	type Todo = {
		id: number;
		text: string;
		completed: boolean;
	};

	// State management with runes
	let todos = $state<Todo[]>([
		{ id: 1, text: 'Learn Svelte 5', completed: false },
		{ id: 2, text: 'Build a Todo App', completed: false },
		{ id: 3, text: 'Master Runes', completed: false }
	]);

	let newTodoText = $state('');

	// Derived values
	let activeTodos: Todo[] = $derived(todos.filter((todo) => !todo.completed));
	let completedTodos: Todo[] = $derived(todos.filter((todo) => todo.completed));

	// Function to add a new todo
	function addTodo() {
		if (newTodoText.trim()) {
			const newTodo: Todo = {
				id: Date.now(),
				text: newTodoText,
				completed: false
			};
			todos = [...todos, newTodo];
			newTodoText = '';
		}
	}

	// Function to toggle a todo's completion status
	function toggleTodo(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	}

	// Function to delete a todo
	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}

	// Handle Enter key press
	function handleKeyPress(e: KeyboardEvent) {
		if (e.key === 'Enter') {
			addTodo();
		}
	}
</script>

<div class="mx-auto mt-8 max-w-md rounded-lg bg-white p-6 shadow-md">
	<h1 class="mb-4 text-2xl font-bold text-gray-800">Todo App</h1>

	<!-- Form to add a new todo -->
	<div class="mb-4 flex">
		<input
			type="text"
			bind:value={newTodoText}
			placeholder="Add a new todo..."
			class="flex-grow rounded-l-lg border px-4 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			onkeypress={handleKeyPress}
		/>
		<button
			onclick={addTodo}
			class="rounded-r-lg bg-blue-500 px-4 py-2 text-white transition-colors hover:bg-blue-600"
		>
			Add
		</button>
	</div>

	<!-- Todo List -->
	<div class="space-y-2">
		{#each todos as todo (todo.id)}
			<div class="group flex items-center rounded-lg border p-3 hover:bg-gray-50">
				<input
					type="checkbox"
					checked={todo.completed}
					onchange={() => toggleTodo(todo.id)}
					class="h-5 w-5 rounded text-blue-500 focus:ring-blue-500"
				/>
				<span
					class="ml-3 flex-grow {todo.completed ? 'text-gray-500 line-through' : 'text-gray-800'}"
				>
					{todo.text}
				</span>
				<button
					onclick={() => deleteTodo(todo.id)}
					class="text-red-500 opacity-0 transition-opacity group-hover:opacity-100 hover:text-red-700"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-5 w-5"
						viewBox="0 0 20 20"
						fill="currentColor"
					>
						<path
							fill-rule="evenodd"
							d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
							clip-rule="evenodd"
						/>
					</svg>
				</button>
			</div>
		{/each}
	</div>

	<!-- Stats -->
	<div class="mt-4 text-sm text-gray-600">
		<p>{activeTodos.length} items left</p>
	</div>
</div>
