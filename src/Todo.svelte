<script>
  export let todos; // STORE
  export let todo;

  // 수정 판별
  let isEdit = false;
  let title = '';

  function onEdit() {
    isEdit = true;
    title = todo.title;
  }

  function onUpdate() {
    todo.title = title;
    $todos = $todos;
    onOffEdit();
  }

  function onOffEdit() {
    isEdit = false;
  }

  function onDeleteTodo() {
    $todos = $todos.filter(t => t.id !== todo.id)
    console.log($todos)
  }
</script>

<main>

  {#if isEdit}
    <input bind:value={title} type="text" on:keydown={(e) => e.key === 'Enter' && onUpdate()} />
    <button type="button" on:click={onUpdate}>OK</button>
    <button type="button" on:click={onOffEdit}>Cancel</button>
    {:else}
    <div>
      - {todo.title}
      <button type="button" on:click={onEdit}>Edit</button>
      <button type="button" on:click={onDeleteTodo}>Delete</button>
    </div>
  {/if}

</main>