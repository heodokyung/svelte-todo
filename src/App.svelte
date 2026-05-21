<script>
  import { writable } from 'svelte/store'
  import Todo from './Todo.svelte'

  let title = ''
  let nextId = 1

  const todos = writable([])

  $: completedCount = $todos.filter((todo) => todo.done).length
  $: remainingCount = $todos.length - completedCount

  function createTodo() {
    const trimmedTitle = title.trim()

    if (!trimmedTitle) {
      title = ''
      return
    }

    todos.update((items) => [
      {
        id: nextId,
        title: trimmedTitle,
        done: false,
      },
      ...items,
    ])

    title = ''
    nextId += 1
  }

  function toggleTodo(todoId) {
    todos.update((items) =>
      items.map((todo) =>
        todo.id === todoId
          ? {
              ...todo,
              done: !todo.done,
            }
          : todo
      )
    )
  }

  function updateTodo(todoId, nextTitle) {
    const trimmedTitle = nextTitle.trim()

    if (!trimmedTitle) return

    todos.update((items) =>
      items.map((todo) =>
        todo.id === todoId
          ? {
              ...todo,
              title: trimmedTitle,
            }
          : todo
      )
    )
  }

  function deleteTodo(todoId) {
    todos.update((items) => items.filter((todo) => todo.id !== todoId))
  }

  function clearCompleted() {
    todos.update((items) => items.filter((todo) => !todo.done))
  }
</script>

<main class="todo-page">
  <section class="todo-card" aria-labelledby="todo-title">
    <div class="todo-card__header">
      <p class="eyebrow">Svelte Study</p>
      <h1 id="todo-title">Todo List</h1>
      <p class="intro">
        Svelte의 상태 관리, 이벤트 처리, 조건부 렌더링을 연습하기 위한 작은 Todo 예제입니다.
      </p>
    </div>

    <div class="todo-summary" aria-label="Todo 통계">
      <div>
        <strong>{$todos.length}</strong>
        <span>전체</span>
      </div>
      <div>
        <strong>{remainingCount}</strong>
        <span>진행 중</span>
      </div>
      <div>
        <strong>{completedCount}</strong>
        <span>완료</span>
      </div>
    </div>

    <div class="todo-form">
      <label class="sr-only" for="todo-input">새 Todo 입력</label>
      <input
        id="todo-input"
        bind:value={title}
        type="text"
        placeholder="오늘 할 일을 입력하세요"
        on:keydown={(event) => event.key === 'Enter' && createTodo()}
      />

      <button class="primary-button" type="button" on:click={createTodo}>
        추가
      </button>
    </div>

    {#if $todos.length}
      <div class="todo-toolbar">
        <p>목록을 클릭해서 완료 상태를 바꾸거나, 내용을 수정할 수 있습니다.</p>

        {#if completedCount}
          <button class="ghost-button" type="button" on:click={clearCompleted}>
            완료 항목 정리
          </button>
        {/if}
      </div>

      <ul class="todo-list" aria-label="Todo 목록">
        {#each $todos as todo (todo.id)}
          <Todo
            {todo}
            onToggle={toggleTodo}
            onUpdate={updateTodo}
            onDelete={deleteTodo}
          />
        {/each}
      </ul>
    {:else}
      <div class="empty-state">
        <div class="empty-state__icon" aria-hidden="true">✓</div>
        <strong>아직 등록된 Todo가 없습니다.</strong>
        <p>가볍게 하나 추가해보고 Svelte의 반응형 흐름을 확인해보세요.</p>
      </div>
    {/if}
  </section>
</main>

<style>
  .todo-page {
    min-height: 100vh;
    padding: 48px 16px;
  }

  .todo-card {
    width: min(100%, 680px);
    margin: 0 auto;
    padding: 32px;
    border: 1px solid rgba(148, 163, 184, 0.22);
    border-radius: 28px;
    background:
      linear-gradient(135deg, rgba(255, 255, 255, 0.94), rgba(248, 250, 252, 0.88)),
      #ffffff;
    box-shadow: 0 24px 70px rgba(15, 23, 42, 0.13);
  }

  .todo-card__header {
    margin-bottom: 24px;
  }

  .eyebrow {
    margin: 0 0 8px;
    color: #2563eb;
    font-size: 13px;
    font-weight: 800;
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }

  h1 {
    margin: 0;
    color: #111827;
    font-size: clamp(34px, 7vw, 56px);
    line-height: 1;
    letter-spacing: -0.06em;
  }

  .intro {
    max-width: 520px;
    margin: 16px 0 0;
    color: #64748b;
    font-size: 15px;
    line-height: 1.7;
  }

  .todo-summary {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-bottom: 20px;
  }

  .todo-summary > div {
    padding: 16px;
    border: 1px solid #e5e7eb;
    border-radius: 18px;
    background: #f8fafc;
  }

  .todo-summary strong {
    display: block;
    color: #111827;
    font-size: 28px;
    line-height: 1;
  }

  .todo-summary span {
    display: block;
    margin-top: 6px;
    color: #64748b;
    font-size: 13px;
    font-weight: 700;
  }

  .todo-form {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 10px;
    margin-bottom: 18px;
  }

  input {
    min-width: 0;
    height: 48px;
    padding: 0 16px;
    border: 1px solid #dbe3ef;
    border-radius: 16px;
    background: #ffffff;
    color: #111827;
    font-size: 15px;
    outline: none;
    transition:
      border-color 0.2s ease,
      box-shadow 0.2s ease;
  }

  input:focus {
    border-color: #2563eb;
    box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.14);
  }

  button {
    border: 0;
    border-radius: 16px;
    font-weight: 800;
    transition:
      transform 0.18s ease,
      box-shadow 0.18s ease,
      background 0.18s ease;
  }

  button:hover {
    transform: translateY(-1px);
  }

  .primary-button {
    min-width: 86px;
    height: 48px;
    padding: 0 18px;
    background: #2563eb;
    color: #ffffff;
    box-shadow: 0 10px 22px rgba(37, 99, 235, 0.22);
  }

  .primary-button:hover {
    background: #1d4ed8;
  }

  .ghost-button {
    min-height: 34px;
    padding: 0 12px;
    background: #eef2ff;
    color: #3730a3;
  }

  .todo-toolbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    margin-bottom: 12px;
  }

  .todo-toolbar p {
    margin: 0;
    color: #64748b;
    font-size: 13px;
    line-height: 1.5;
  }

  .todo-list {
    display: grid;
    gap: 10px;
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .empty-state {
    padding: 34px 20px;
    border: 1px dashed #cbd5e1;
    border-radius: 22px;
    background: #f8fafc;
    color: #64748b;
    text-align: center;
  }

  .empty-state__icon {
    display: grid;
    width: 46px;
    height: 46px;
    margin: 0 auto 14px;
    place-items: center;
    border-radius: 999px;
    background: #dbeafe;
    color: #2563eb;
    font-size: 22px;
    font-weight: 900;
  }

  .empty-state strong {
    display: block;
    color: #111827;
    font-size: 17px;
  }

  .empty-state p {
    margin: 8px 0 0;
    font-size: 14px;
    line-height: 1.6;
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
    .todo-page {
      padding: 24px 12px;
    }

    .todo-card {
      padding: 22px;
      border-radius: 22px;
    }

    .todo-summary {
      grid-template-columns: 1fr;
    }

    .todo-form {
      grid-template-columns: 1fr;
    }

    .primary-button {
      width: 100%;
    }

    .todo-toolbar {
      align-items: flex-start;
      flex-direction: column;
    }
  }
</style>
