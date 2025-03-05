<script lang="ts">
	// Define Todo interface
	interface Todo {
		id: number;
		text: string;
		completed: boolean;
	}

	// State using Svelte 5 $state rune
	let todos = $state<Todo[]>([
		{ id: 1, text: 'Learn Svelte 5', completed: false },
		{ id: 2, text: 'Build a Todo app', completed: false }
	]);
	let newTodoText = $state('');

	// Derived state using $derived rune
	let completedCount = $derived(todos.filter((todo) => todo.completed).length);
	let remainingCount = $derived(todos.length - completedCount);

	// Methods
	function addTodo() {
		if (newTodoText.trim()) {
			const newId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;
			todos = [...todos, { id: newId, text: newTodoText.trim(), completed: false }];
			newTodoText = '';
		}
	}

	function toggleTodo(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	}

	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}

	// Side effect when todos change
	$effect(() => {
		console.log('Todos updated:', todos);
	});
</script>

<div class="max-w-md mx-auto my-8 p-6 bg-white rounded-lg shadow-lg">
	<h1 class="text-2xl font-bold text-gray-800 mb-6">Svelte 5 Todo App</h1>

	<!-- Add new todo -->
	<div class="flex mb-4">
		<input
			type="text"
			placeholder="Add a new todo..."
			class="flex-grow px-4 py-2 border rounded-l-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
			bind:value={newTodoText}
			onkeydown={(e) => e.key === 'Enter' && addTodo()}
		/>
		<button
			class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-r-lg transition-colors"
			onclick={addTodo}
		>
			Add
		</button>
	</div>

	<!-- Stats -->
	<div class="flex justify-between text-sm text-gray-600 mb-4">
		<span>{remainingCount} remaining</span>
		<span>{completedCount} completed</span>
	</div>

	<!-- Todo list -->
	<ul class="space-y-2">
		{#each todos as todo (todo.id)}
			<li class="flex items-center p-3 border rounded-lg group hover:bg-gray-50 transition-colors">
				<input
					type="checkbox"
					class="w-5 h-5 mr-3 accent-blue-500 cursor-pointer"
					checked={todo.completed}
					onchange={() => toggleTodo(todo.id)}
				/>
				<span class={todo.completed ? 'flex-grow line-through text-gray-400' : 'flex-grow'}>
					{todo.text}
				</span>
				<button
					class="text-red-500 opacity-0 group-hover:opacity-100 transition-opacity px-2 py-1 rounded hover:bg-red-50"
					onclick={() => deleteTodo(todo.id)}
				>
					Delete
				</button>
			</li>
		{/each}

		{#if todos.length === 0}
			<li class="text-center p-4 text-gray-500 italic">No todos yet. Add one above!</li>
		{/if}
	</ul>
</div>
