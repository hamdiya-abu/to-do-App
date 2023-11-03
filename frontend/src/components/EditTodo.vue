<template>
    <div class="container">
    <h2 class="text-gray-800 font-semibold mt-10 mb-4 text-xl flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" height="1em" viewBox="0 0 512 512"><!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M410.3 231l11.3-11.3-33.9-33.9-62.1-62.1L291.7 89.8l-11.3 11.3-22.6 22.6L58.6 322.9c-10.4 10.4-18 23.3-22.2 37.4L1 480.7c-2.5 8.4-.2 17.5 6.1 23.7s15.3 8.5 23.7 6.1l120.3-35.4c14.1-4.2 27-11.8 37.4-22.2L387.7 253.7 410.3 231zM160 399.4l-9.1 22.7c-4 3.1-8.5 5.4-13.3 6.9L59.4 452l23-78.1c1.4-4.9 3.8-9.4 6.9-13.3l22.7-9.1v32c0 8.8 7.2 16 16 16h32zM362.7 18.7L348.3 33.2 325.7 55.8 314.3 67.1l33.9 33.9 62.1 62.1 33.9 33.9 11.3-11.3 22.6-22.6 14.5-14.5c25-25 25-65.5 0-90.5L453.3 18.7c-25-25-65.5-25-90.5 0zm-47.4 168l-144 144c-6.2 6.2-16.4 6.2-22.6 0s-6.2-16.4 0-22.6l144-144c6.2-6.2 16.4-6.2 22.6 0s6.2 16.4 0 22.6z"/></svg>
        Edit Todo</h2>
    <form @submit="updateTodo">
        <div>
  <label for="title" class="block text-sm font-medium text-gray-700">Title:</label>
  <input v-model="editedTodo.title" id="title" name="title" required
    class="w-full p-2 border border-gray-300 rounded-md focus:ring focus:ring-indigo-200 focus:outline-none"
  >
</div>

<div class="mt-2">
  <label for="description" class="block text-sm font-medium text-gray-700">Description:</label>
  <textarea v-model="editedTodo.description" id="description" name="description"
    class="w-full p-4 border border-gray-300 rounded-md focus:ring focus:ring-indigo-200 focus:outline-none resize-y"
  ></textarea>
</div>

<div class="flex items-center space-x-1">
  <input type="checkbox" v-model="editedTodo.is_complete" id="isComplete" name="isComplete"
    class="text-indigo-600 border-gray-300 rounded focus:ring focus:ring-indigo-200"
  >
  <label for="isComplete" class="text-md text-gray-700">Completed</label>
</div>

<div class="mt-4 justify-between space-x-4 ">  
    <button type="submit" @click="updateTodo" class="bg-blue-500 text-white py-2 px-5 rounded">Update</button>
    <button @click.prevent="cancel" class="text-white py-2 px-5 bg-red-400 rounded">Cancel</button>

</div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  data() {
    return {
      editedTodo: {
        title: '',
        description: '',
        is_complete: false,
      },
    };
  },
  methods: {
    async fetchTodoData() {
      try {
        const todoId = this.$route.params.id; 
        const response = await axios.get(`http://127.0.0.1:8000/api/todo/${todoId}`); 
        this.editedTodo = response.data; 
      } catch (error) {
        console.error('Error fetching todo data:', error);
        
      }
    },
     updateTodo() {
      try {
        const todoId = this.$route.params.id; 
         axios.put(`http://127.0.0.1:8000/api/todo/${todoId}/`, this.editedTodo); 
        this.$router.push({ path: '/' });
        console.log()
    } catch (error) {
        console.error('Error updating todo:', error);
        
    }
    },
    cancel() {
      
      this.$router.push('/'); 
    },
  },
  mounted() {
    this.fetchTodoData(); 
  },
};
</script>

<style scoped>

</style>

