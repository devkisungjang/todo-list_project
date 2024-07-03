<template>
  <v-list-item-content>
    <v-list-item-title :class="{ completed: todo.completed }">{{
      todo.text
    }}</v-list-item-title>
    <v-list-item-action>
      <v-btn icon @click="$emit('toggleComplete')">
        <v-icon>{{
          todo.completed ? "mdi-check-circle" : "mdi-circle-outline"
        }}</v-icon>
      </v-btn>
      <v-btn icon @click="$emit('deleteTodo')">
        <v-icon color="red">mdi-delete</v-icon>
      </v-btn>
    </v-list-item-action>
  </v-list-item-content>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>
