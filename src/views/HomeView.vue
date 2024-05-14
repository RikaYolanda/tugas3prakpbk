<template>
  <div class="todo-app">
    <h1>Kegiatan Hari ini</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Add a task..." />
      <button type="submit" class="primary">Add</button>
    </form>
    <h2>List Items</h2>
    <div class="filter-section">
      <button @click="filterTodos('all')" class="filter-btn">All</button>
      <button @click="filterTodos('active')" class="filter-btn">Not Completed</button>
      <button @click="filterTodos('completed')" class="filter-btn">Completed</button>
    </div>
    <ul class="todo-list">
  <li v-for="(todo, index) in filteredTodos" :key="index">
    <input type="checkbox" v-model="todo.done" />
    <span :class="{ 'done': todo.done, 'editing': todo.editing }" @dblclick="editTodo(index)">{{ todo.text }}</span>
    <input v-if="todo.editing" type="text" v-model="editedTodoText" @blur="saveEditedTodo(index)" @keyup.enter="saveEditedTodo(index)" @keyup.esc="cancelEdit(index)" />
    <button @click="removeTodo(index)" class="danger">Remove</button>
    <button @click="editTodo(index)" class="edit">Edit</button> <!-- Tambahkan tombol edit -->
  </li>
</ul>
    <footer>
      <p>Created by yola</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
      editedTodoText: '',
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        text: this.newTodo,
        done: false,
        editing: false
      });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    filterTodos(filterType) {
      this.filter = filterType;
    },
    editTodo(index) {
      this.editedTodoText = this.todos[index].text;
      this.todos[index].editing = true;
    },
    saveEditedTodo(index) {
      if (this.editedTodoText.trim().length === 0) {
        this.todos.splice(index, 1);
      } else {
        this.todos[index].text = this.editedTodoText;
        this.todos[index].editing = false;
      }
      this.editedTodoText = '';
    },
    cancelEdit(index) {
      this.todos[index].editing = false;
      this.editedTodoText = '';
    }
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'active':
          return this.todos.filter(todo => !todo.done);
        case 'completed':
          return this.todos.filter(todo => todo.done);
        default:
          return this.todos;
      }
    },
  },
};
</script>

<style>
/* General Styles */
body {
  color: black;
  font-family: "Dancing Script", cursive;
  margin: 0;
  background-image: url(/src/assets/aes.jpeg);
  background: cover;
  background-size: cover;
  background-repeat: no-repeat;
}

/* Todo List Container */
.todo-app {
  max-width: 600px;
  margin: 40px auto;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

/* Heading */
.todo-app h1 {
  text-align: center;
  color: #9933cc;
  font-size: 2rem;
  margin-bottom: 20px;
}

/* Input Form */
.todo-app form {
  display: flex;
  margin-bottom: 20px;
}

.todo-app input[type=text] {
  flex: 1;
  padding: 15px 20px;
  font-size: 1.2rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  outline: none;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.todo-app button[type=submit] {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 1.2rem;
  background-color: #e0b2ff;
  color: #fff;
}

.todo-list li {
  position: relative;
}

.todo-list input[type=checkbox] {
  position: absolute;
  left: -40px;
}

.todo-list .editing {
  display: none;
}

.todo-list .editing + button {
  display: none;
}
</style>
