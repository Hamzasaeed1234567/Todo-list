<script>
  let todos = [];
  let newTodo = "";


  function addTodo() {
    if (newTodo.trim()) {
      todos = [...todos, { text: newTodo }];
      newTodo = "";
    }
  }


function deleteTodo(index) {
  todos = todos.filter((todo, i) => i !== index);
}

</script>

<style>

</style>

<h1>To-Do List</h1>


<input
  type="text"
  bind:value={newTodo}
  placeholder="Add a new value..."
/>
<button on:click={addTodo}>Add</button>


<ul>
  {#each todos as todo, index}
    <li>
  
      <span >{todo.text}</span>
      <button on:click={() => deleteTodo(index)}>Delete</button>
    </li>
  {/each}
</ul>
