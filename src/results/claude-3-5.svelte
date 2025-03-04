<script lang="ts">
	import { let } from 'svelte/runes';

	type Todo = {
		id: number;
		text: string;
		completed: boolean;
	};

	let todos = $state<Todo[]>([]);
	let newTodoText = $state('');
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
</script>

<div class="mx-auto max-w-lg p-6">
	<h1 class="mb-6 text-3xl font-bold text-gray-800">Todo List</h1>

	<div class="mb-6 flex gap-2">
		<input
			type="text"
			bind:value={newTodoText}
			placeholder="Add a new todo..."
			class="flex-1 rounded-lg border px-4 py-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			on:keydown={(e) => e.key === 'Enter' && addTodo()}
		/>
		<button
			on:click={addTodo}
			class="rounded-lg bg-blue-500 px-4 py-2 text-white transition-colors hover:bg-blue-600"
		>
			Add
		</button>
	</div>

	<ul class="space-y-3">
		{#each todos as todo (todo.id)}
			<li class="flex items-center gap-3 rounded-lg border bg-white p-3 shadow-sm">
				<input
					type="checkbox"
					checked={todo.completed}
					on:change={() => toggleTodo(todo.id)}
					class="h-5 w-5 rounded text-blue-500 focus:ring-blue-500"
				/>
				<span class="flex-1 {todo.completed ? 'text-gray-500 line-through' : 'text-gray-800'}">
					{todo.text}
				</span>
				<button
					on:click={() => deleteTodo(todo.id)}
					class="text-red-500 transition-colors hover:text-red-700"
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
			</li>
		{/each}
	</ul>

	{#if todos.length === 0}
		<p class="mt-6 text-center text-gray-500">No todos yet. Add one above!</p>
	{/if}
</div>
