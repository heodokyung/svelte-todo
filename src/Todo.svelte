<script>
  export let todo
  export let onToggle
  export let onUpdate
  export let onDelete

  let isEditing = false
  let draftTitle = ''

  function startEdit() {
    isEditing = true
    draftTitle = todo.title
  }

  function cancelEdit() {
    isEditing = false
    draftTitle = ''
  }

  function saveEdit() {
    if (!draftTitle.trim()) return

    onUpdate(todo.id, draftTitle)
    cancelEdit()
  }
</script>

<li class:done={todo.done}>
  {#if isEditing}
    <div class="edit-row">
      <label class="sr-only" for={`todo-edit-${todo.id}`}>Todo 수정</label>
      <input
        id={`todo-edit-${todo.id}`}
        bind:value={draftTitle}
        type="text"
        on:keydown={(event) => event.key === 'Enter' && saveEdit()}
      />

      <button class="save-button" type="button" on:click={saveEdit}>저장</button>
      <button class="text-button" type="button" on:click={cancelEdit}>취소</button>
    </div>
  {:else}
    <div class="view-row">
      <button
        class="check-button"
        type="button"
        aria-label={todo.done ? 'Todo 미완료로 변경' : 'Todo 완료로 변경'}
        on:click={() => onToggle(todo.id)}
      >
        {#if todo.done}
          ✓
        {/if}
      </button>

      <button class="todo-title" type="button" on:click={() => onToggle(todo.id)}>
        {todo.title}
      </button>

      <div class="actions">
        <button class="text-button" type="button" on:click={startEdit}>수정</button>
        <button class="danger-button" type="button" on:click={() => onDelete(todo.id)}>
          삭제
        </button>
      </div>
    </div>
  {/if}
</li>

<style>
  li {
    border: 1px solid #e5e7eb;
    border-radius: 18px;
    background: #ffffff;
    box-shadow: 0 10px 24px rgba(15, 23, 42, 0.05);
    transition:
      border-color 0.18s ease,
      transform 0.18s ease,
      box-shadow 0.18s ease;
  }

  li:hover {
    border-color: #bfdbfe;
    transform: translateY(-1px);
    box-shadow: 0 14px 28px rgba(15, 23, 42, 0.08);
  }

  li.done {
    background: #f8fafc;
  }

  .view-row,
  .edit-row {
    display: grid;
    align-items: center;
    gap: 10px;
    padding: 12px;
  }

  .view-row {
    grid-template-columns: auto 1fr auto;
  }

  .edit-row {
    grid-template-columns: 1fr auto auto;
  }

  button {
    border: 0;
    font: inherit;
    cursor: pointer;
  }

  input {
    min-width: 0;
    height: 42px;
    padding: 0 12px;
    border: 1px solid #dbe3ef;
    border-radius: 14px;
    color: #111827;
    outline: none;
  }

  input:focus {
    border-color: #2563eb;
    box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.14);
  }

  .check-button {
    display: grid;
    width: 32px;
    height: 32px;
    place-items: center;
    border: 2px solid #cbd5e1;
    border-radius: 999px;
    background: #ffffff;
    color: #ffffff;
    font-size: 17px;
    font-weight: 900;
  }

  li.done .check-button {
    border-color: #2563eb;
    background: #2563eb;
  }

  .todo-title {
    overflow: hidden;
    padding: 0;
    background: transparent;
    color: #111827;
    font-size: 15px;
    font-weight: 800;
    line-height: 1.5;
    text-align: left;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  li.done .todo-title {
    color: #94a3b8;
    text-decoration: line-through;
  }

  .actions {
    display: flex;
    gap: 6px;
  }

  .text-button,
  .danger-button,
  .save-button {
    min-height: 34px;
    padding: 0 12px;
    border-radius: 12px;
    font-size: 13px;
    font-weight: 800;
  }

  .text-button {
    background: #f1f5f9;
    color: #334155;
  }

  .text-button:hover {
    background: #e2e8f0;
  }

  .danger-button {
    background: #fff1f2;
    color: #e11d48;
  }

  .danger-button:hover {
    background: #ffe4e6;
  }

  .save-button {
    background: #2563eb;
    color: #ffffff;
  }

  .save-button:hover {
    background: #1d4ed8;
  }

  .sr-only {
    position: absolute;
    overflow: hidden;
    width: 1px;
    height: 1px;
    margin: -1px;
    padding: 0;
    border: 0;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
  }

  @media (max-width: 560px) {
    .view-row {
      grid-template-columns: auto 1fr;
    }

    .actions {
      grid-column: 1 / -1;
      justify-content: flex-end;
    }

    .edit-row {
      grid-template-columns: 1fr;
    }

    .save-button,
    .text-button,
    .danger-button {
      width: 100%;
    }
  }
</style>
