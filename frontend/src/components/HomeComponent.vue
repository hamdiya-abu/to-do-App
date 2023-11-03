<template>
  <div class="container">
    <h1 class="text-gray-600 font-semibold text-xl mt-10">Create A Todo</h1>
    <div class="flex flex-col mt-4 gap-2">
      <input
        v-model="newTodo.title"
        placeholder="Title"
        class="p-2 border border-gray-300 rounded-md focus:ring focus:ring-indigo-200 focus:outline-none"
      />
      <h1 class="text-sm mt-2 text-gray-700 font-medium">
        What's on your todo list?
      </h1>
      <textarea
        class="p-4 border border-gray-300 rounded-md focus:ring focus:ring-indigo-200 focus:outline-none resize-y"
        v-model="newTodo.description"
        placeholder="Description"
      ></textarea>
      <label>
        <input type="checkbox" v-model="newTodo.is_completed" />
        Is Completed
      </label>
      <div>
        <button
          class="bg-blue-500 text-white p-3 rounded font-semibold hover:bg-blue-600 mt-2"
          @click="addTodo"
        >
          Add Todo
        </button>
      </div>
    </div>
    <hr class="mt-5 border" />

    <h1 class="mt-10 text-gray-800 font-medium">Todo List</h1>
    <ul class="mt-2 space-y-4">
      <li
        v-for="todo in todos"
        :key="todo.id"
        class="bg-[#FFF] p-3 rounded-lg flex justify-between shadow-md"
      >
        <div class="flex gap-2">
          <div>
            <input type="checkbox" v-model="todo.is_completed" />
          </div>
          <div>
            <strong>{{ todo.title }}</strong>
            <p>{{ todo.description }}</p>
          </div>
        </div>

        <div class="space-x-2 text-white font-semibold flex mobile:flex-col mobile:space-y-2  sm:flex-row">
          <router-link
            :to="`/todos/${todo.id}/edit`"
            class="bg-blue-500 hover:bg-blue-600  py- px-2 rounded gap-1 mobile:px-1 flex items-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="24"
              fill="white"
              viewBox="0 0 24 24"
            >
              <path
                d="M17.65 6.35l-1.79 1.79A6.978 6.978 0 0012 2a7 7 0 000 14 6.978 6.978 0 004.94-2.06l1.79 1.79A9.005 9.005 0 0112 22 9 9 0 012 12a9.003 9.003 0 013.65-7.65l-1.79-1.79A6.978 6.978 0 0012 2a7 7 0 000 14 6.978 6.978 0 004.94-2.06zM12 20c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8z"
              />
            </svg>

            Edit</router-link
          >
          <button
            @click="deleteTodo(todo)"
            class="bg-red-400 p-2  rounded flex gap-1 items-center"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="1.1em"
              viewBox="0 0 448 512"
            >
              <!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
              <path
                d="M135.2 17.7C140.6 6.8 151.7 0 163.8 0H284.2c12.1 0 23.2 6.8 28.6 17.7L320 32h96c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 96 0 81.7 0 64S14.3 32 32 32h96l7.2-14.3zM32 128H416V448c0 35.3-28.7 64-64 64H96c-35.3 0-64-28.7-64-64V128zm96 64c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16z"
                fill="white"
              />
            </svg>

            Delete
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      newTodo: {
        title: "",
        description: "",
        is_completed: false,
      },
      todos: [],
    };
  },
  created() {
    this.fetchTodos();
  },
  methods: {
    async fetchTodos() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/api/todo/");
        this.todos = response.data;
      } catch (error) {
        console.error("Error fetching todos:", error);
      }
    },
    async addTodo() {
      try {
        const response = await axios.post(
          "http://127.0.0.1:8000/api/todo/",
          this.newTodo
        );
        this.todos.push(response.data);
        this.newTodo = {
          title: "",
          description: "",
          is_completed: false,
        };
      } catch (error) {
        console.error("Error adding todo:", error);
      }
    },
    async editTodo(todo) {
      // Implement edit functionality
    },
    async deleteTodo(todo) {
      try {
        await axios.delete(`http://127.0.0.1:8000/api/todo/${todo.id}/`);
        this.todos = this.todos.filter((t) => t.id !== todo.id);
      } catch (error) {
        console.error("Error deleting todo:", error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>