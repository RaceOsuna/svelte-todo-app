<script>
// @ts-nocheck

import {
  initializeApp
} from "firebase/app";
import {
  getFirestore,
  collection,
  onSnapshot,
  doc,
  setDoc, 
  deleteDoc,
  addDoc
} from "firebase/firestore";
import {
  firebaseConfig
} from '$lib/index'

const app = initializeApp(firebaseConfig)
const db = getFirestore(app)

const collect = collection(db, "taskList")

let taskList = []

const unsubscribe = onSnapshot(collect, (snapshot) => {
  let fbtl = []
  snapshot.forEach(item => {
    let task = {
      ...item.data(),
      id: item.id
    }
    fbtl = [task, ...fbtl]
  })
  taskList = fbtl
})

console.log(taskList)

let inputText = ''
let updateText = ''
let taskToUpdate = null
 
const addTask = async () => {
  const docRef = await addDoc(collection(db, "taskList"), {
  name: inputText,
  isComplete: false
});
}

const markComplete = async (task) => {
  await setDoc(doc(db, "taskList", task.id), {
    ...task,
    isComplete: !task.isComplete
  });
}

const deleteTask = async (task) => {
  await deleteDoc(doc(db, "taskList", task.id))
}

const editTask = (task) => {
  updateText = task.name
  taskToUpdate = task
}

const updateTask = async() => {
  await setDoc(doc(db, "taskList", taskToUpdate.id), {
    ...taskToUpdate,
    name: updateText
  });
}
</script>

<main>
    <div class="app-container">
        <h1>Todo 📒</h1>
        <form>
            <input type="text" bind:value={inputText}>
            <button disabled={inputText ? false : true} on:click={addTask}>Add task</button>
            <input type="text" disabled={updateText ? false : true} bind:value={updateText}>
            <button disabled={taskToUpdate ? false : true} on:click={updateTask}>Update</button>
        </form>
        {#if taskList.length}
        <ol>
            {#each taskList as task, index}
            <div class="list-item">
                <li class:complete={task.isComplete}>{task.name}</li>
                <div class="icon-container">
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span role="button" tabindex="0" class="icon" on:click={() => editTask(task)}>✏️</span>
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span role="button" tabindex="0" class="icon" on:click={() => markComplete(task)}>✅</span>
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span role="button" tabindex="0" class="icon" on:click={() => deleteTask(task)}>🗑️</span>
                </div>
            </div>
            {/each}
        </ol>
        {:else}
        <h2>No Tasks</h2>
        {/if}
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
  font-size: x-large;
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
