<template>
  <div id="app">
    <h1>Danh Sách Công Việc</h1>
    <input v-model="newTask" placeholder="Thêm công việc mới" @keyup.enter="addTask" />
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.completed" />
        <span :class="{ completed: task.completed }">{{ task.text }}</span>
        <button @click="removeTask(index)">Xóa</button>
      </li>
    </ul>
    <button @click="clearCompleted">Xóa công việc đã hoàn thành</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    };
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = '';
        this.saveTasks();
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !task.completed);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const tasks = localStorage.getItem('tasks');
      if (tasks) {
        this.tasks = JSON.parse(tasks);
      }
    }
  },
  mounted() {
    this.loadTasks();
  }
};
</script>

<style>
.completed {
  text-decoration: line-through;
}
</style>