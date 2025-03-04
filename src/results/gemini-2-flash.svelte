<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const todos = $state<
		{
			id: number;
			text: string;
			completed: boolean;
		}[]
	>([]);
	const newTodo = $state('');
	let nextId = $state(0);

	function addTodo() {
		if (newTodo.trim() === '') return;
		todos.push({
			id: nextId,
			text: newTodo,
			completed: false
		});
		nextId++;
		newTodo = '';
	}

	function toggleTodo(id: number) {
		todos.forEach((todo) => {
			if (todo.id === id) {
				todo.completed = !todo.completed;
			}
		});
		todos = todos;
	}

	function deleteTodo(id: number) {
		todos.splice(
			todos.findIndex((todo) => todo.id === id),
			1
		);
		todos = todos;
	}
</script>

<div class="flex h-screen items-center justify-center bg-gray-100">
	<div class="w-96 rounded bg-white p-8 shadow-md">
		<h1 class="mb-4 text-2xl font-semibold text-gray-800">Todo App</h1>

		<div class="mb-4 flex">
			<input
				type="text"
				class="focus:shadow-outline w-full appearance-none rounded border px-3 py-2 leading-tight text-gray-700 shadow focus:outline-none"
				placeholder="Add New Todo"
				bind:value={newTodo}
				on:keydown={(e) => {
					if (e.key === 'Enter') {
						addTodo();
					}
				}}
			/>
			<button
				class="focus:shadow-outline ml-2 rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700 focus:outline-none"
				on:click={addTodo}
			>
				Add
			</button>
		</div>

		{#each todos as todo (todo.id)}
			<div class="flex items-center justify-between border-b border-gray-200 py-2">
				<div class="flex items-center">
					<input
						type="checkbox"
						class="mr-2 leading-tight"
						checked={todo.completed}
						on:click={() => toggleTodo(todo.id)}
					/>
					<span class:line-through={todo.completed} class="text-gray-700">
						{todo.text}
					</span>
				</div>
				<button
					class="focus:shadow-outline rounded px-2 py-1 font-bold text-red-500 hover:text-red-700 focus:outline-none"
					on:click={() => deleteTodo(todo.id)}
				>
					Delete
				</button>
			</div>
		{/each}
	</div>
</div>
