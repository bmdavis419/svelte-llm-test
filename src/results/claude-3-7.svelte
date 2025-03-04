<script lang="ts">
	import { $state, $derived } from 'svelte';

	type Todo = {
		id: number;
		text: string;
		completed: boolean;
	};

	let todos = $state<Todo[]>([]);
	let newTodoText = $state('');
	let nextId = $state(1);

	const incompleteTodos = $derived(todos.filter((todo) => !todo.completed));
	const completedTodos = $derived(todos.filter((todo) => todo.completed));

	function addTodo() {
		if (newTodoText.trim() === '') return;

		todos = [
			...todos,
			{
				id: nextId,
				text: newTodoText,
				completed: false
			}
		];

		nextId++;
		newTodoText = '';
	}

	function toggleTodo(id: number) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	}

	function deleteTodo(id: number) {
		todos = todos.filter((todo) => todo.id !== id);
	}
</script>

<div class="mx-auto mt-10 max-w-md rounded-lg bg-white p-6 shadow-md">
	<h1 class="mb-6 text-2xl font-bold text-gray-800">Todo App</h1>

	<form class="mb-6" on:submit|preventDefault={addTodo}>
		<div class="flex gap-2">
			<input
				type="text"
				bind:value={newTodoText}
				placeholder="Add a new todo..."
				class="flex-grow rounded-md border border-gray-300 px-4 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			/>
			<button
				type="submit"
				class="rounded-md bg-blue-500 px-4 py-2 text-white hover:bg-blue-600 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			>
				Add
			</button>
		</div>
	</form>

	<div class="space-y-6">
		{#if incompleteTodos.length > 0}
			<div>
				<h2 class="mb-3 text-lg font-semibold text-gray-700">Tasks</h2>
				<ul class="space-y-2">
					{#each incompleteTodos as todo (todo.id)}
						<li class="flex items-center justify-between rounded-md bg-gray-50 p-3">
							<div class="flex items-center gap-3">
								<input
									type="checkbox"
									checked={todo.completed}
									on:change={() => toggleTodo(todo.id)}
									class="h-5 w-5 rounded text-blue-500 focus:ring-blue-500"
								/>
								<span class="text-gray-800">{todo.text}</span>
							</div>
							<button
								on:click={() => deleteTodo(todo.id)}
								class="text-red-500 hover:text-red-700 focus:outline-none"
								aria-label="Delete todo"
							>
								<svg
									xmlns="http://www.w3.org/2000/svg"
									class="h-5 w-5"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
									/>
								</svg>
							</button>
						</li>
					{/each}
				</ul>
			</div>
		{/if}

		{#if completedTodos.length > 0}
			<div>
				<h2 class="mb-3 text-lg font-semibold text-gray-700">Completed</h2>
				<ul class="space-y-2">
					{#each completedTodos as todo (todo.id)}
						<li class="flex items-center justify-between rounded-md bg-gray-50 p-3">
							<div class="flex items-center gap-3">
								<input
									type="checkbox"
									checked={todo.completed}
									on:change={() => toggleTodo(todo.id)}
									class="h-5 w-5 rounded text-blue-500 focus:ring-blue-500"
								/>
								<span class="text-gray-500 line-through">{todo.text}</span>
							</div>
							<button
								on:click={() => deleteTodo(todo.id)}
								class="text-red-500 hover:text-red-700 focus:outline-none"
								aria-label="Delete todo"
							>
								<svg
									xmlns="http://www.w3.org/2000/svg"
									class="h-5 w-5"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
									/>
								</svg>
							</button>
						</li>
					{/each}
				</ul>
			</div>
		{/if}

		{#if todos.length === 0}
			<p class="py-4 text-center text-gray-500">No todos yet. Add one above!</p>
		{/if}
	</div>
</div>
