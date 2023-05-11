<script setup>
import {computed, ref} from "vue";

  let todoList = ref([]);
  const newTodoName = ref('');
  const todoId = ref(0);

  const readLocalStorage = () => {
    const data = localStorage.getItem('todoList');
    if (data) {
      todoList.value = JSON.parse(data);
      todoId.value = todoList.value.length;
    }
  };

  const writeLocalStorage = () => {
    localStorage.setItem('todoList', JSON.stringify(todoList.value));
  };

  const addTodo = () => {
    todoList.value.push({
      id: todoId.value++,
      name: newTodoName.value || 'New Todo',
      done: false
    });
    newTodoName.value = '';
    writeLocalStorage();
  };

  const removeTodo = (id) => {
    todoList.value.splice(todoList.value.findIndex(todo => todo.id === id), 1);
    writeLocalStorage();
  };


  const toggleTodo = (id) => {
    todoList.value = todoList.value.map(todo => {
      if (todo.id === id) {
        todo.done = !todo.done;
      }
      return todo;
    });
    writeLocalStorage()
  };
 
  const completedTodoNum = computed(() => {
    return todoList.value.filter(todo => todo.done).length;
  });

  readLocalStorage();

</script>

<template>
  <top>
      <up></up>
      <header>        
          <div id ="nav">
            <router-link to="/">Home</router-link>|
            <router-link :to="{ name: 'About' }">About</router-link>|
            <router-link :to="{ name: 'Todo' }">Todo</router-link>          
            </div>
          <router-view/>
          <h2>Self Reminder!</h2>
          <div id="todoStatus">
              <div id="totalComp">
                  <span id="totalCompleted" class="countData">{{completedTodoNum}}</span><span class="countName"> Total Terselesaikan</span>
              </div>
              <div id="totalNum">
                  <span id="totalTodos" class="countData">{{todoList.length}}</span><span class="countName"> Selesai</span>
              </div>
          </div>
      </header>
      <main>
          <div id="todoInput">
              <div class="listItem">
                  <input type="text" v-model="newTodoName" placeholder="Masukkan kegiatan positifmu..." @keydown.enter="addTodo" @keydown.esc="removeTodo(todoList[0].id)">
              </div>
          </div>
          <div id="todoList" v-for="todo in todoList" :key="todo.id">
              <div class="listItem">
                  <div v-if="todo.done" class="todoNameDone" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <div v-else class="todoName" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <button @click="removeTodo(todo.id)">×</button>
              </div>
          </div>

      </main>
  </top>
</template>

<style>
:root {
    --color-background: grey;
    --color-main: #DCCDD1;

    --font-size-extra-large: 90px;
    --font-size-large: 60px;
    --font-size-medium: 20px;

    --global-radius: 15px;
    --global-margin: 10px;
    --side-margin: 20px;

    --max-width: 300px;
    --min-width: 500px;
    --common-width: 60%;
}

* {
    background-color: grey;
    background-size: auto;
    color: black;
    font-family: "arial";
    border: 0;
    padding: 0;
    margin: 0;;
    user-select: none;
}

top {
    width: var(--common-width);
    min-width: var(--min-width);
    max-width: var(--max-width);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

up {
    height: 240px;
}

header {
    background-color: var(--color-background);
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
}

main {
    display: flex;
    flex-direction: column;
}

h2 {
    font-size: var(--font-size-extra-large);
    font-weight: bold;
}

.countData {
    font-size: var(--font-size-large);
    font-weight: bold;
}

.countName {
    font-size: var(--font-size-medium);
    font-weight: normal;
}



input {
    border: none;
    outline: none;
    font-size: var(--font-size-small);
    background-color: inherit;
    width: 100%;
}

button {
    border: none;
    font-size: var(--font-size-medium);
    background-color: inherit;
    color: burlywood;
}



.listItem {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    background-color: var(--color-main);
    margin: var(--global-margin) var(--side-margin);
}

#todoList, #todoInput {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: stretch;
    background-color: var(--color-main);
    border-radius: var(--global-radius);
    padding: var(--global-margin);
    margin-top: var(--global-margin);
    margin-bottom: var(--global-margin);
}


.todoName, .todoNameDone {
    font-size: var(--font-size-medium);
    background-color: inherit;
}

.todoNameDone {
    text-decoration: line-through;
}

#app {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>