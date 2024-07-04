<template>
  <v-list>
    <v-list-item v-for="(todo, index) in todos" :key="index" class="todo-item">
      <v-list-item-content class="d-flex w-100">
        <v-list-item-title :class="{ completed: todo.completed }">
          <v-text-field
            v-if="editingTodo === todo"
            v-model="todo.title"
            @keyup.enter="saveTodo"
            outlined
            ref="editInput"
            full-width
          ></v-text-field>
          <span v-else>{{ todo.title }}</span>
        </v-list-item-title>
        <div class="action-buttons">
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                @click="$emit('toggle-complete', todo)"
                v-bind="attrs"
                v-on="on"
              >
                <v-icon color="green">{{
                  todo.completed
                    ? "mdi-check-circle-outline"
                    : "mdi-checkbox-blank-circle-outline"
                }}</v-icon>
              </v-btn>
            </template>
            <span>{{
              todo.completed ? "Mark as Incomplete" : "Mark as Complete"
            }}</span>
          </v-tooltip>
          <v-tooltip top v-if="!todo.completed">
            <template v-slot:activator="{ on, attrs }">
              <v-btn icon @click="editTodo(todo)" v-bind="attrs" v-on="on">
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
            </template>
            <span>Edit</span>
          </v-tooltip>
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                @click="$emit('delete-todo', todo)"
                v-bind="attrs"
                v-on="on"
              >
                <v-icon color="red">mdi-delete</v-icon>
              </v-btn>
            </template>
            <span>Delete</span>
          </v-tooltip>
        </div>
      </v-list-item-content>
    </v-list-item>
  </v-list>
</template>

<script>
export default {
  name: "TodoList",
  props: {
    todos: Array,
  },
  data() {
    return {
      editingTodo: null,
    };
  },
  methods: {
    editTodo(todo) {
      this.editingTodo = todo;
      this.$nextTick(() => {
        this.$refs.editInput[0].focus();
      });
      this.$emit("edit-todo", todo);
    },
    saveTodo() {
      if (this.editingTodo) {
        this.$emit("update-todo", this.editingTodo);
        this.editingTodo = null;
      }
    },
  },
};
</script>

<style scoped>
.todo-item {
  border-bottom: 1px solid #e0e0e0;

  width: 100%;
  height: 100px;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.v-list-item-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.v-list-item-title {
  margin-left: 16px;
  flex-grow: 1;
  display: flex;
  align-items: center;
}

.action-buttons {
  display: flex;
  align-items: center;
  gap: 8px;
}
</style>
