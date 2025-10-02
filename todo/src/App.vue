<template>
  <div align="center" style="margin-top: 50px">
    <form @submit.prevent="createTodo">
      <label for="todo-description">Description</label>
      <input v-model="data.todoDescription" type="text" id="todo-description" @keyup.ctrl.enter="createTodo" />

      <input type="submit" value="Create" />
      <br /><br />
      <label for="todo-color">Color</label>
      <input type="color" id="todo-color" v-model="data.todoColor" />
    </form>

    <ol style="text-align: left; max-width: 400px; margin-top: 20px">
      <li v-for="(todo, index) in data.todos" :key="index">
        <!-- color box -->
        <span
          :style="{
            backgroundColor: todo.color,
            display: 'inline-block',
            width: '15px',
            height: '15px',
            borderRadius: '50%',
            marginRight: '10px',
          }"
        ></span>

        <template v-if="todo.isEditing">
          <input v-model="data.todos[index].description" type="text" />
          <button @click="saveTodo(index)">Save</button>
        </template>
        <template v-else>
          <del v-if="todo.isCompleted">{{ todo.description }}</del>
          <span v-else>{{ todo.description }}</span>

          <button @click="deleteTodo(index)">Delete 🗑️</button>
          <button v-if="!todo.isCompleted" @click="markAsComplete(index)">
            Complete ✅
          </button>
          <button @click="editTodo(index)">Edit ✏️</button>
        </template>
      </li>
    </ol>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const data = reactive({
  todos: [],
  todoDescription: "",
  todoColor: "#000000"
});

function createTodo() {
  data.todos.push({
    description: data.todoDescription,
    isCompleted: false,
    isEditing: false,
    color: data.todoColor,
  });
  data.todoDescription = "";
}

function deleteTodo(index) {
  data.todos.splice(index, 1);
}

function markAsComplete(index) {
  data.todos[index].isCompleted = true;
}

function editTodo(index) {
  data.todos[index].isEditing = true;
  const newDescription = data.todos[index].description;
  if (newDescription !== null) {
    data.todos[index].description = newDescription;
  }
}

function saveTodo(index) {
  data.todos[index].isEditing = false;
}
</script>
