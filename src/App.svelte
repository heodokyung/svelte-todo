<script>
	import { writable } from 'svelte/store'
	import Todo from './Todo.svelte'

	let title = '';
	let todos = writable([]);
	let id = 0;

	function createTodo () {

		if( !title.trim()) {
			title = '';
			return
		}

		$todos.push({
			id,
			title
		});

		$todos = $todos;
		title = '';
		id += 1;
	}

	// 아래의 3가지 코드는 모두 동일함.
	// if( e.key === 'Enter') { createTodo() }
	// e.key === 'Enter' ? createTodo() : undefined
	// e.key === 'Enter' && createTodo()
</script>

<main>
	<input bind:value={title} type="text" on:keydown={(e) => {e.key === 'Enter' && createTodo()}} />
	<button type="button" on:click={createTodo}>Create Todo</button>

	{#each $todos as todo}
		<!-- <Todo todos={todos} todo={todo} /> -->
		<Todo {todos} {todo} />
	{/each}
</main>

<style>
	main {
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>