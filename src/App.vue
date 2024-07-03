<template>
  <v-app>
    <v-container>
      <v-row justify="center">
        <v-col cols="12" md="8">
          <v-card class="pa-5">
            <v-card-title class="text-center">
              <h2>TODO LIST</h2>
            </v-card-title>
            <v-card-text>
              <todo-form @add-todo="addTodo" />
              <v-row justify="center" class="mb-3">
                <v-btn-toggle v-model="filter" mandatory>
                  <v-btn value="all">All</v-btn>
                  <v-btn value="active">Active</v-btn>
                  <v-btn value="completed">Completed</v-btn>
                </v-btn-toggle>
              </v-row>
              <todo-list :todos="filteredTodos" @toggle-complete="toggleComplete" @delete-todo="deleteTodo" @edit-todo="editTodo" @update-todo="updateTodo"/>
              <p v-if="!todos.length" class="text-center">추가할 항목이 없습니다</p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import TodoForm from './components/TodoForm.vue';
import TodoList from './components/TodoList.vue';
import TodoItem from './components/TodoItem.vue';

export default {
  name: 'App',
  components: {
    TodoForm,
    TodoList,
    TodoItem,
  },
  data() {
    return {
      todos: [],
      filter: 'all',
      editingTodo: null,
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    }
  },
  methods: {
    addTodo(todo) {
      this.todos.push(todo);
    },
    toggleComplete(todo) {
      todo.completed = !todo.completed;
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter(t => t !== todo);
    },
    editTodo(todo) {
      this.editingTodo = todo;
    },
    updateTodo(updatedTodo) {
      const index = this.todos.findIndex(todo => todo === this.editingTodo);
      if (index !== -1) {
        this.todos.splice(index, 1, updatedTodo);
        this.editingTodo = null;
      }
    }
  },
};
</script>

<style>
h2 {
  color: #3f51b5;
  font-family: 'Arial', sans-serif;
}

.text-center {
  text-align: center;
}

.v-card {
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.v-btn-toggle .v-btn {
  border-radius: 8px;
}
</style>