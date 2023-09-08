<script>
  let taskList = [
    {name: 'test1', isComplete: false}, 
    {name: 'test2', isComplete: true},
    {name: 'test3', isComplete: false}
]

let inputText = ''

const addTask = () => {
  let newTask = {
  name: inputText,
  isComplete: false
};
  taskList = [...taskList, newTask];
  inputText = ''
}

</script>

<main>
  <div>
    <h1>Todo 📒</h1>
    <form action="">
      <input type="text" bind:value={inputText}>
      <button on:click={addTask}>Add task</button>
    </form>
    <ol>
      {#each taskList as task}
      <li class:complete={task.isComplete}>{task.name}</li>
      {/each}
    </ol>
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .complete {
    text-decoration: line-through;
  }

  button {
    cursor: pointer;
  }
</style>