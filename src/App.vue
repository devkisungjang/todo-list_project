<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col>
          <v-card>
            <v-card-title class="primary--text">
              ToDo List
            </v-card-title>
            <v-card-text>
              <v-form v-model="valid">
                <v-text-field
                  label="New Task"
                  v-model="newTask"
                  :rules="[v => !!v || 'Task name is required']"
                  @keyup.enter="addTask"
                  color="primary"
                ></v-text-field>
                <v-btn color="primary" @click="addTask" :disabled="!valid">Add Task</v-btn>
              </v-form>
            </v-card-text>
            <v-divider></v-divider>
            <v-list>
              <v-list-item
                v-for="(task, index) in tasks"
                :key="index"
                @click="toggleComplete(index)"
                :class="{'completed-task': task.completed}"
              >
                <v-list-item-content>
                  <v-text-field
                    v-model="task.text"
                    @blur="updateTask(index)"
                    single-line
                    :readonly="task.completed"
                    color="primary"
                  ></v-text-field>
                </v-list-item-content>
                <v-list-item-action v-if="task.completed">
                  <v-chip color="primary" class="white--text">완료!</v-chip>
                </v-list-item-action>
                <v-list-item-action>
                  <v-btn icon @click.stop="deleteTask(index)">
                    <v-icon color="red">mdi-delete</v-icon>
                  </v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-list>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      valid: false,
      newTask: '',
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === '') return;
      this.tasks.push({ text: this.newTask, completed: false });
      this.newTask = '';
    },
    updateTask(index) {
      if (this.tasks[index].text.trim() === '') {
        this.deleteTask(index);
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
  },
};
</script>

<style>
.primary--text {
  color: #00AEEF !important;
}
.completed-task .v-text-field input {
  text-decoration: line-through;
  color: grey;
}
.completed-task {
  opacity: 0.6;
}
.v-btn.primary {
  background-color: #00AEEF !important;
}
</style>
