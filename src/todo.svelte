<script>
  let todos = [];
  let newTodo = "";
  let category = "General";
  let priority = "Low";
  let isEditing = false;
  let currentEditIndex = null;
  let selectedTodos = [];
  let toggleAllSelectboolean = false;
  let isLoading = false;
  function addTodo() {
    if (newTodo.trim()) {
      isLoading = true; // Show loader
      setTimeout(() => {
        if (isEditing && currentEditIndex !== null) {
          todos[currentEditIndex] = { text: newTodo, category, priority };
          isEditing = false;
          currentEditIndex = null;
        } else {
          todos = [...todos, { text: newTodo, category, priority }];
        }
        newTodo = "";
        category = "General";
        priority = "Low";
        toggleAllSelectboolean = false;
        isLoading = false; // Hide loader after processing
      }, 1000); // Simulate delay
    }
  }
  function deleteTodo(index) {
    todos = todos.filter((_, i) => i !== index);
    selectedTodos = selectedTodos.filter((i) => i !== index);
    if (todos.length === 0) {
      toggleAllSelectboolean = false;
    }
  }
  function editTodo(index) {
    const todo = todos[index];
    newTodo = todo.text;
    category = todo.category;
    priority = todo.priority;
    isEditing = true;
    currentEditIndex = index;
  }
  function toggleSelect(index) {
    if (selectedTodos.includes(index)) {
      selectedTodos = selectedTodos.filter((i) => i !== index);
      toggleAllSelectboolean = false;
    } else {
      selectedTodos = [...selectedTodos, index];
    }
  }
  function toggleAllSelect() {
    if (selectedTodos.length === todos.length) {
      selectedTodos = [];
    } else {
      selectedTodos = todos.map((_, index) => index);
      toggleAllSelectboolean = true;
    }
  }
  function deleteSelectedTodos() {
    todos = todos.filter((_, i) => !selectedTodos.includes(i));
    selectedTodos = [];
    if (todos.length === 0) {
      toggleAllSelectboolean = false;
    }
  }
</script>
<style>
.loader {
    text-align: center;
    color: #007BFF;
    font-weight: bold;
    margin-top: 10px;
  }
  .container {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    max-width: 400px;
    width: 100%;
    margin-top: 20px;
  }
  h1 {
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  form {
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  input[type="text"] {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }
  select {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }
  .priority-group {
    display: flex;
    justify-content: space-between;
  }
  .priority-group label {
    font-size: 14px;
    color: #555;
  }
  .buttons {
    display: flex;
    justify-content: space-between;
    gap: 10px;
  }
  button {
    padding: 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
  }
  button:hover {
    opacity: 0.9;
  }
  button[type="submit"] {
    background-color: #007BFF;
    color: white;
  }
  button[type="button"] {
    background-color: #DC3545;
    color: white;
  }
  ul {
    list-style: none;
    padding: 0;
    margin-top: 20px;
  }
  li {
    background: #F4F4F4;
    margin: 10px 0;
    padding: 10px;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  li label {
    flex-grow: 1;
  }
  li button {
    margin-left: 10px;
    font-size: 14px;
  }
  p {
    text-align: center;
    color: #333;
    font-size: 14px;
  }
</style>
<div class="container">
  <h1>To-Do List</h1>
  {#if isLoading}
    <p class="loader">Processing...</p>
  {/if}
  <form on:submit|preventDefault={addTodo}>
    <input
      type="text"
      bind:value={newTodo}
      placeholder="Add a new task..."
      disabled={isLoading}
    />
    <select bind:value={category} disabled={isLoading}>
      <option value="General">General</option>
      <option value="Work">Work</option>
      <option value="Personal">Personal</option>
    </select>
    <div class="priority-group">
      <label>
        <input type="radio" bind:group={priority} value="Low" disabled={isLoading} /> Low
      </label>
      <label>
        <input type="radio" bind:group={priority} value="Medium" disabled={isLoading} /> Medium
      </label>
      <label>
        <input type="radio" bind:group={priority} value="High" disabled={isLoading} /> High
      </label>
    </div>
    <div class="buttons">
      <button type="submit" disabled={isLoading}>{isEditing ? "Save" : "Add"}</button>
      {#if selectedTodos.length > 0}
        <button type="button" on:click={deleteSelectedTodos} disabled={isLoading}>Delete Selected</button>
      {/if}
    </div>
  </form>
  <ul>
    {#if selectedTodos.length > 0}
      <p>{selectedTodos.length} items selected</p>
    {/if}
    {#if todos.length > 0}
      <label>
        Select All
        <input
          type="checkbox"
          checked={toggleAllSelectboolean}
          on:change={() => toggleAllSelect()}
          disabled={isLoading}
        />
      </label>
    {/if}
    {#each todos as todo, index}
      <li>
        <label>
          <input
            type="checkbox"
            checked={selectedTodos.includes(index)}
            on:change={() => toggleSelect(index)}
            disabled={isLoading}
          />
          {todo.text} - {todo.category} ({todo.priority})
        </label>
        <button on:click={() => deleteTodo(index)} disabled={isLoading}>Delete</button>
        <button on:click={() => editTodo(index)} disabled={isLoading}>Edit</button>
      </li>
    {/each}
  </ul>
</div>