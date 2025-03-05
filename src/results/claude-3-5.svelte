<script lang="ts">
	interface Todo {
		id: number;
		text: string;
		completed: boolean;
	}

	let todos = $state<Todo[]>([]);
	let newTodoText = $state('');

	// Generate unique IDs for todos
	let nextId = $state(1);

	function addTodo() {
		if (newTodoText.trim()) {
			todos = [
				...todos,
				{
					id: nextId,
					text: newTodoText.trim(),
					completed: false
				}
			];
			nextId++;
			newTodoText = '';
		}
	}

	function toggleTodo(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	}

	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}

	// Derived values for stats
	let completedCount = $derived(todos.filter((t) => t.completed).length);
	let totalCount = $derived(todos.length);
</script>

<div class="mx-auto max-w-lg rounded-lg bg-white p-6 shadow-lg">
	<h1 class="mb-6 text-3xl font-bold text-gray-800">Todo List</h1>

	<!-- Add new todo form -->
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
			placeholder="What needs to be done?"
			class="flex-1 rounded-lg border px-4 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
		/>
		<button
			type="submit"
			class="rounded-lg bg-blue-500 px-4 py-2 text-white hover:bg-blue-600 focus:ring-2 focus:ring-blue-500 focus:outline-none"
		>
			Add
		</button>
	</form>

	<!-- Stats -->
	<div class="mb-4 text-sm text-gray-600">
		{completedCount} of {totalCount} tasks completed
	</div>

	<!-- Todo list -->
	<ul class="space-y-2">
		{#each todos as todo (todo.id)}
			<li
				class="flex items-center gap-3 rounded-lg bg-gray-50 p-3 transition-colors hover:bg-gray-100"
			>
				<input
					type="checkbox"
					checked={todo.completed}
					onclick={() => toggleTodo(todo.id)}
					class="h-5 w-5 rounded border-gray-300 text-blue-500 focus:ring-blue-500"
				/>
				<span class={todo.completed ? 'flex-1 text-gray-500 line-through' : 'flex-1'}>
					{todo.text}
				</span>
				<button
					onclick={() => deleteTodo(todo.id)}
					class="text-red-500 hover:text-red-700 focus:outline-none"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-5 w-5"
						viewBox="0 0 20 20"
						fill="currentColor"
					>
						<path
							fill-rule="evenodd"
							d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
							clip-rule="evenodd"
						/>
					</svg>
				</button>
			</li>
		{/each}
	</ul>

	{#if todos.length === 0}
		<p class="mt-6 text-center text-gray-500">No todos yet. Add one above!</p>
	{/if}
</div>
