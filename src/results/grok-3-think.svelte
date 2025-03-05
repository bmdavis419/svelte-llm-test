<script lang="ts">
	// Define the Todo interface for TypeScript typing
	interface Todo {
		id: number;
		description: string;
		done: boolean;
	}

	// Reactive state using Svelte 5's $state rune
	let todos = $state<Todo[]>([]);
	let nextId = $state(1);
	let newTodoDescription = $state('');

	// Function to add a new todo
	function addTodo() {
		if (newTodoDescription.trim() !== '') {
			todos.push({ id: nextId++, description: newTodoDescription.trim(), done: false });
			newTodoDescription = ''; // Clear the input after adding
		}
	}

	// Function to delete a todo by ID
	function deleteTodo(id: number) {
		todos = todos.filter((t) => t.id !== id);
	}
</script>

<!-- HTML structure with Tailwind styling -->
<div class="mx-auto max-w-md p-4">
	<h1 class="mb-4 text-2xl font-bold">Todo App</h1>

	<!-- Input and Add button section -->
	<div class="mb-4 flex space-x-2">
		<input
			bind:value={newTodoDescription}
			placeholder="What needs to be done?"
			class="flex-1 rounded border p-2"
			onkeydown={(e) => e.key === 'Enter' && addTodo()}
		/>
		<button
			onclick={addTodo}
			disabled={newTodoDescription.trim() === ''}
			class="rounded bg-blue-500 p-2 text-white disabled:bg-gray-300"
		>
			Add
		</button>
	</div>

	<!-- Todo list or empty message -->
	{#if todos.length === 0}
		<p class="text-gray-500">No todos yet.</p>
	{:else}
		<ul class="space-y-2">
			{#each todos as todo (todo.id)}
				<li class="flex items-center space-x-2">
					<input type="checkbox" bind:checked={todo.done} class="h-4 w-4" />
					<span class={todo.done ? 'text-gray-500 line-through' : ''}>
						{todo.description}
					</span>
					<button onclick={() => deleteTodo(todo.id)} class="p-1 text-red-500 hover:text-red-700">
						Delete
					</button>
				</li>
			{/each}
		</ul>
	{/if}
</div>
