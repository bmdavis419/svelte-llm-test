<script lang="ts">
	interface Todo {
		id: number;
		text: string;
		completed: boolean;
	}

	let todos = $state<Todo[]>([
		{ id: 1, text: 'Learn Svelte 5', completed: false },
		{ id: 2, text: 'Build a todo app', completed: true }
	]);

	let newTodoText = $state('');

	const addTodo = () => {
		if (newTodoText.trim()) {
			todos = [...todos, { id: Date.now(), text: newTodoText, completed: false }];
			newTodoText = '';
		}
	};

	const toggleTodo = (id: number) => {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));
	};

	const deleteTodo = (id: number) => {
		todos = todos.filter((todo) => todo.id !== id);
	};
</script>

<div class="flex min-h-screen items-center justify-center bg-gray-100">
	<div class="w-full max-w-md rounded bg-white p-8 shadow-md">
		<h1 class="mb-4 text-center text-2xl font-bold">Todo List</h1>

		<div class="mb-4 flex">
			<input
				type="text"
				placeholder="Add a new todo..."
				class="mr-2 w-full rounded border px-3 py-2"
				bind:value={newTodoText}
				on:keydown={(e) => {
					if (e.key === 'Enter') {
						addTodo();
					}
				}}
			/>
			<button
				class="rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
				onclick={addTodo}
			>
				Add
			</button>
		</div>

		<ul>
			{#each todos as todo (todo.id)}
				<li class="flex items-center justify-between border-b py-2 last:border-b-0">
					<div class="flex items-center">
						<input
							type="checkbox"
							class="mr-2"
							checked={todo.completed}
							on:change={() => toggleTodo(todo.id)}
						/>
						<span class={todo.completed ? 'text-gray-500 line-through' : ''}>
							{todo.text}
						</span>
					</div>
					<button class="text-red-500 hover:text-red-700" onclick={() => deleteTodo(todo.id)}>
						Delete
					</button>
				</li>
			{/each}
		</ul>
	</div>
</div>
