<script lang="ts">
	// State for todos
	interface Todo {
		id: number;
		text: string;
		done: boolean;
	}

	let todos = $state<Todo[]>([
		{ id: 1, text: 'Grocery Shopping', done: false },
		{ id: 2, text: 'Walk the dog', done: true },
		{ id: 3, text: 'Learn Svelte 5', done: false }
	]);

	// State for new todo input
	let newTodoText = $state('');

	// Function to add a new todo
	const addTodo = () => {
		if (newTodoText.trim() !== '') {
			todos.push({
				id: Date.now(), // Simple unique ID
				text: newTodoText,
				done: false
			});
			newTodoText = ''; // Clear input after adding
		}
	};

	// Function to toggle todo completion
	const toggleDone = (id: number) => {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, done: !todo.done } : todo));
	};

	// Function to delete a todo
	const deleteTodo = (id: number) => {
		todos = todos.filter((todo) => todo.id !== id);
	};
</script>

<div class="flex min-h-screen flex-col justify-center bg-gray-100 py-6 sm:py-12">
	<div class="relative py-3 sm:mx-auto sm:max-w-xl">
		<div
			class="absolute inset-0 -skew-y-6 transform rounded-3xl bg-gradient-to-r from-cyan-400 to-sky-500 shadow-lg sm:-rotate-6 sm:skew-y-0"
		/>
		<div class="relative rounded-3xl bg-white px-4 py-10 shadow-lg sm:p-20">
			<div class="mx-auto max-w-md">
				<div>
					<h1 class="text-center text-2xl font-semibold text-gray-800">My Todo List</h1>
				</div>

				<div class="divide-y divide-gray-200">
					<div class="space-y-4 py-8 text-base leading-6 text-gray-700 sm:text-lg sm:leading-7">
						{#each todos as todo (todo.id)}
							<div class="flex items-center justify-between py-2">
								<div class="flex items-center">
									<input
										type="checkbox"
										class="form-checkbox h-5 w-5 cursor-pointer rounded border-gray-300 text-cyan-600 focus:ring-cyan-500"
										checked={todo.done}
										onclick={() => toggleDone(todo.id)}
									/>
									<label class="ml-3 text-gray-900 {todo.done ? 'text-gray-500 line-through' : ''}">
										{todo.text}
									</label>
								</div>
								<button
									class="text-red-500 hover:text-red-700 focus:outline-none"
									onclick={() => deleteTodo(todo.id)}
									aria-label="Delete Todo"
								>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										fill="none"
										viewBox="0 0 24 24"
										stroke-width="1.5"
										stroke="currentColor"
										class="h-5 w-5"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L16.5 5.09m-1.602 3.934l-.799.494m-0.783-.5A3.5 3.5 0 0 1 17 8.599V12a.75.75 0 0 1-.75.75H5.25a.75.75 0 0 1-.75-.75V8.599c0-.214.02-.429.06-.634m.034-.265l-0.783-.5m-0.283.5a3.5 3.5 0 0 1 3.5 3.5H17m-5 0v-3.5a1.5 1.5 0 0 0-1.5-1.5H9.5a1.5 1.5 0 0 0-1.5 1.5V12m-8.457 3.077l-.017.001-.007.003-.005.001-.003.001-.001.001a.752.752 0 0 1-.704.752.752.752 0 0 1-.752-.704.752.752 0 0 1 .704-.752.752.752 0 0 1 .752.704l.001.001.003.001.005.001.007.003.017.001m8.457-3.077v-3.5a1.5 1.5 0 0 0-1.5-1.5H9.5a1.5 1.5 0 0 0-1.5 1.5V12"
										/>
									</svg>
								</button>
							</div>
						{/each}
					</div>
					<div class="space-y-4 pt-6 text-base leading-6 font-bold sm:text-lg sm:leading-7">
						<div class="flex space-x-4 pt-4">
							<input
								type="text"
								placeholder="Add new todo"
								class="block w-full rounded-md border-gray-300 shadow-sm focus:border-cyan-500 focus:ring-cyan-500 sm:text-sm"
								bind:value={newTodoText}
								onkeydown={(event) => {
									if (event.key === 'Enter') {
										addTodo();
									}
								}}
							/>
							<button
								type="button"
								class="inline-flex items-center rounded-md border border-transparent bg-cyan-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-cyan-700 focus:ring-2 focus:ring-cyan-500 focus:ring-offset-2 focus:outline-none"
								onclick={addTodo}
							>
								Add
							</button>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
```
