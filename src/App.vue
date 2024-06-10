<template>
  <div id="app" class="container">
    <h1>Daftar Tugas</h1>
    <div class="input-container">
      <input v-model="newTask" @keyup.enter="addNewTask" placeholder="Tambahkan Tugas" />
      <button @click="addNewTask" class="add-button">Tambahkan</button>
    </div>
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        <div class="task-text">
          <input type="checkbox" v-model="task.completed" @change="toggleTaskStatus(index)" />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
        </div>
        <button @click="removeTask(index)" class="remove-button">Hapus</button>
      </li>
    </ul>
    <p class="incomplete-tasks">Incomplete tasks: {{ incompleteTasksCount }}</p>
  </div>
</template>

<script>
import { useTodoStore } from './stores/todoStore';
import { ref } from 'vue';

export default {
  setup() {
    const store = useTodoStore();
    const newTask = ref('');

    const addNewTask = () => {
      if (newTask.value.trim() !== '') {
        store.addTask(newTask.value);
        newTask.value = '';
      }
    };

    const removeTask = (index) => {
      store.removeTask(index);
    };

    const toggleTaskStatus = (index) => {
      store.toggleTaskStatus(index);
    };

    return {
      newTask,
      tasks: store.tasks,
      addNewTask,
      removeTask,
      toggleTaskStatus,
      incompleteTasksCount: store.incompleteTasksCount
    };
  }
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f7f7f7;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  width: 400px;
  max-width: 100%;
}

h1 {
  text-align: center;
  color: #333;
}

.input-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

input {
  width: 65%;
  padding: 10px;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
}

.add-button {
  width: 30%;
  padding: 10px;
  background-color: #28a745;
  border: none;
  border-radius: 5px;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #218838;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.task-item:last-child {
  border-bottom: none;
}

.task-text {
  display: flex;
  align-items: center;
  flex: 1;
}

input[type="checkbox"] {
  margin-right: 10px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.remove-button {
  background-color: #dc3545;
  border: none;
  border-radius: 5px;
  color: white;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.remove-button:hover {
  background-color: #c82333;
}

.incomplete-tasks {
  text-align: center;
  color: #666;
  font-size: 14px;
  margin-top: 20px;
}
</style>
