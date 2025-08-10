<script>
	import { onMount } from 'svelte';
	import { v4 as uuidv4 } from 'uuid';

	let todos = [];
	let newTodo = '';

	let isBrowser = false;

	onMount(() => {
		isBrowser = true;
		const saved = localStorage.getItem('todos');
		if (saved) {
			todos = JSON.parse(saved).map((todo) => ({
				id: todo.id ?? uuidv4(),
				text: todo.text,
				done: todo.done
			}));
			saveTodos();
		}
	});

	$: if (isBrowser) {
		saveTodos();
	}

	function saveTodos() {
		localStorage.setItem('todos', JSON.stringify(todos));
	}

	function addTodo() {
		const trimmed = newTodo.trim();
		if (trimmed === '') return;

		if (todos.some((t) => t.text.toLowerCase() === trimmed.toLowerCase())) {
			alert('すでに同じ内容のタスクがあります');
			return;
		}

		todos = [...todos, { id: uuidv4(), text: trimmed, done: false }];
		newTodo = '';
	}

	function toggleDone(id) {
		todos = todos.map((todo) => (todo.id === id ? { ...todo, done: !todo.done } : todo));
	}

	function deleteTodo(id) {
		todos = todos.filter((todo) => todo.id !== id);
	}

	function clearAll() {
		if (confirm('本当にすべてのタスクを削除しますか？')) {
			todos = [];
		}
	}
</script>

<h1>TODOリスト</h1>

<div class="input-area">
	<input
		type="text"
		bind:value={newTodo}
		placeholder="新しいタスクを入力"
		on:keydown={(e) => e.key === 'Enter' && addTodo()}
	/>
	<button on:click={addTodo}>追加</button>
	<button class="clear-btn" on:click={clearAll} disabled={todos.length === 0}>すべて削除</button>
</div>

<ul>
	{#each todos as todo (todo.id)}
		<li>
			<div>
				<input type="checkbox" checked={todo.done} on:change={() => toggleDone(todo.id)} />
				<span class:done={todo.done}>{todo.text}</span>
			</div>
			<button on:click={() => deleteTodo(todo.id)}>削除</button>
		</li>
	{/each}
</ul>

<style>
	h1 {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		color: #333;
	}

	.input-area {
		margin-bottom: 1em;
		display: flex;
		gap: 0.5em;
	}

	input[type='text'] {
		flex-grow: 1;
		padding: 0.5em 0.75em;
		font-size: 1rem;
		border: 2px solid #6596e3;
		border-radius: 8px;
		outline-offset: 2px;
		transition: border-color 0.2s;
	}

	input[type='text']:focus {
		border-color: #f49a2a;
	}

	button {
		background-color: #6596e3;
		color: white;
		border: none;
		border-radius: 8px;
		padding: 0.5em 1em;
		font-size: 1rem;
		cursor: pointer;
		transition: background-color 0.2s;
		user-select: none;
	}

	button:hover {
		background-color: #4a74c1;
	}

	button:active {
		background-color: #3560a3;
	}

	.clear-btn {
		background-color: #f49a2a;
	}

	.clear-btn:hover {
		background-color: #c9781e;
	}

	.clear-btn:active {
		background-color: #9e5d18;
	}

	ul {
		list-style-type: none;
		padding-left: 0;
		margin-top: 1em;
	}

	li {
		margin-bottom: 0.6em;
		display: flex;
		align-items: center;
		gap: 0.5em;
		background: #f0f4ff;
		padding: 0.5em 1em;
		border-radius: 10px;
		box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
	}

	li span {
		flex-grow: 1;
		font-size: 1rem;
		user-select: none;
	}

	.done {
		text-decoration: line-through;
		color: #888;
	}

	li button {
		margin-left: auto;
		background-color: #d9534f;
		padding: 0 12px;
		font-size: 0.9rem;
		border-radius: 6px;

		height: 32px;
		line-height: 32px;
		white-space: nowrap;
		display: inline-block;

		user-select: none;
	}

	li button:hover {
		background-color: #c9302c;
	}

	li button:active {
		background-color: #ac2925;
	}

	@media (max-width: 600px) {
		.input-area {
			flex-direction: column;
			gap: 0.5em;
		}

		input[type='text'] {
			width: 100%;
			max-width: 100%;
			font-size: 1.1rem;
			padding: 0.75em 1em;
			box-sizing: border-box;
		}

		button {
			width: 100%;
			padding: 0.75em 0;
			font-size: 1.1rem;
		}

		ul li {
			flex-direction: column;
			align-items: flex-start;
			gap: 0.3em;
			padding: 0.6em 1em;
		}

		ul li > div {
			display: flex;
			align-items: center;
			width: 100%;
			gap: 0.5em;
		}

		ul li input[type='checkbox'] {
			flex-shrink: 0;
			width: 24px;
			height: 24px;
			cursor: pointer;
		}

		ul li span {
			flex-grow: 1;
			font-size: 1.1rem;
			word-break: break-word;
		}

		ul li > button {
			width: 100%;
			padding: 0.4em 0.7em;
			font-size: 1rem;
			background-color: #d9534f;
			border-radius: 6px;
			margin-top: 0;
			align-self: stretch;
			user-select: none;

			display: flex;
			align-items: center;
			justify-content: center;

			height: 32px;
			line-height: 32px;
			white-space: nowrap;
		}

		ul li > button:hover {
			background-color: #c9302c;
		}

		ul li > button:active {
			background-color: #ac2925;
		}
	}
</style>
