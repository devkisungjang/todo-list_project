<template>
  <v-form @submit.prevent="onSubmit">
    <v-row align="center">
      <v-col cols="12">
        <v-text-field
          v-model="title"
          label="오늘은 어떤 일을 하실 건가요?"
          outlined
          clearable
        ></v-text-field>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>
export default {
  name: "TodoForm",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    onSubmit() {
      if (this.title.trim()) {
        this.$emit("add-todo", { title: this.title, completed: false });
        this.title = "";
      }
    },
  },
};
</script>

<style scoped>
.v-form {
  margin-bottom: 20px;
}
</style>
