<template>
  <div class="todo-app">
    <h1>To-Do</h1>
    
    <div class="input-row">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Новая задача"
      >
      <button @click="addTask">+</button>
    </div>
    
    <ul class="tasks">
      <li
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ done: task.completed }"
      >
        <span @click="toggleTask(index)">{{ task.text }}</span>
        <button @click="removeTask(index)">×</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    }
  },
  created() {
    const saved = localStorage.getItem('tasks')
    if (saved) this.tasks = JSON.parse(saved)
  },
  methods: {
    addTask() {
      if (!this.newTask.trim()) return
      this.tasks.push({ text: this.newTask, completed: false })
      this.newTask = ''
      this.save()
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
      this.save()
    },
    toggleTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed
      this.save()
    },
    save() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }
  }
}
</script>

<style>
body {
  font-family: sans-serif;
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.todo-app {
  text-align: center;
}

h1 {
  font-weight: normal;
  margin: 0 0 20px 0;
}

.input-row {
  display: flex;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
}

button {
  padding: 8px 12px;
  border: 1px solid #ddd;
  background: none;
  cursor: pointer;
}

.tasks {
  list-style: none;
  padding: 0;
  margin: 0;
  text-align: left;
}

.tasks li {
  padding: 8px 0;
  border-bottom: 1px solid #eee;
  display: flex;
}

.tasks li span {
  flex: 1;
  cursor: pointer;
}

.tasks li.done span {
  text-decoration: line-through;
  color: #999;
}

.tasks li button {
  padding: 0 5px;
}
</style>