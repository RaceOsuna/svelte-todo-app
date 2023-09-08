<script>
  let taskList = [
    {name: 'test1', isComplete: false}, 
    {name: 'test2', isComplete: false},
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

// @ts-ignore
const markComplete = (index) => {
  if(!taskList[index].isComplete) {
  taskList[index].isComplete = true
  } else {
    taskList[index].isComplete = false
  }
}

// @ts-ignore
const deleteTask = (index) => {
  const deleteTask = taskList[index]
  taskList = taskList.filter(task => task !== deleteTask)
}

</script>

<main>
  <div class="app-container">
    <h1>Todo 📒</h1>
    <form action="">
      <input type="text" bind:value={inputText}>
      <button on:click={addTask}>Add task</button>
    </form>
    <ol>
      {#each taskList as task, index}
      <div class="list-item">
        <li class:complete={task.isComplete}>{task.name}</li>
        <div class="icon-container">
          <span class="icon">✏️</span>
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <span role="button" tabindex="0" class="icon" on:click={() => markComplete(index)}>✅</span>
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <span role="button" tabindex="0" class="icon" on:click={() => deleteTask(index)}>🗑️</span>
        </div>
      </div>
      {/each}
    </ol>
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    margin: 0;
  }

  .app-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 50%;
  }
  .complete {
    text-decoration: line-through;
    color: red;
  }

  ol {
    width: 100%;
    padding-left: 0;
  }

  .list-item {
    display: flex;
  }
  li {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }

  .icon-container {
    display: flex;
    justify-content: space-around;
    width: 30%;
  }

  button {
    cursor: pointer;
  }

  .icon {
    cursor: pointer;
  }
</style>