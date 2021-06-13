<template>
<div class="container bg-white shadow rounded-3 m-5 p-5 mx-auto w-75">
    <h1 class="fw-bold text-center fs-1 text-primary">todo <i class="fas fa-tasks"></i></h1>
    <form class="d-flex text-center bg-light p-4 rounded-3 align-items-center mt-3 mb-5 shadow-sm" @submit.prevent="addTodo()">
      <div class="input-group">
        <input
            v-model="newTodo"
            name="newTodo"
            autocomplete="off"
            class="flex-grow-1 form-control fs-4"
        >
        <button class="btn btn-primary fs-4">add it</button>
      </div>
    </form>
    <h2 class="fw-bold text-end text-primary text-secondary">my <i class="fas fa-tasks"></i> list</h2>
    <ul class="list-group fs-3">
        <li v-for="(todo, index) in todos" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
          <span :class="{ done: todo.done }" @click="doneTodo(todo)">
            {{ todo.content }}
          </span>
          <button class="btn btn-danger" @click="removeTodo(index)"><i class="fas fa-dumpster-fire"></i></button>
        </li>
    </ul>
    <h4 class="mt-2 mb-4 text-center text-secondary fw-bold bg-light p-3 rounded-3 shadow-sm" v-if="todos.length === 0">your todo list is empty.</h4>
</div>
</template>

<script>
//importing bootstrap 5
import "bootstrap/dist/css/bootstrap.min.css";
import '@fortawesome/fontawesome-free/js/all.js';

    import { ref } from 'vue';
    export default {
        name: 'App',
        setup () {
            const newTodo = ref('');
            const defaultData = [{
                done: false,
                content: 'Read about Vue'
            }]
            const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
            const todos = ref(todosData);
            function addTodo () {
                if (newTodo.value) {
                    todos.value.push({
                        done: false,
                        content: newTodo.value
                    });
                    newTodo.value = '';
                }
                saveData();
            }
            function doneTodo (todo) {
                todo.done = !todo.done
                saveData();
            }
            function removeTodo (index) {
                todos.value.splice(index, 1);
                saveData();
            }
            function saveData () {
                const storageData = JSON.stringify(todos.value);
                localStorage.setItem('todos', storageData);
            }
            return {
                todos,
                newTodo,
                addTodo,
                doneTodo,
                removeTodo,
                saveData
            }
        }
    }
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600;700;800&family=Poppins:wght@300;400;600;700;800&display=swap');

body { font-family: 'Open Sans', sans-serif; }
h1, h2, h3, h4, h5, h6 { font-family: 'Poppins', sans-serif; }

.open-sans { font-family: 'Open Sans', sans-serif; }
.poppins { font-family: 'Poppins', sans-serif; }

.done { text-decoration: line-through; }
</style>
