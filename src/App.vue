<template>
  <div id="app">
    <div class="glassmorphism">
    <h1>Tarefas</h1>
    <tasks-progress :progress="progress"/>
    <new-task @taskAdded="addTask" />
    <task-grid :tasks="tasks" @taskDeleted="deleteTask" @taskStateChanged="toggleTaskState"></task-grid>
    </div>
  </div>
</template>

<script>
import TaskGrid from '@/components/TaskGrid'
import NewTask from '@/components/NewTask'
import TasksProgress from '@/components/TasksProgress'

export default {
  components: {
    TaskGrid,
    NewTask,
    TasksProgress,
  },
  watch: {
    tasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
  },
  computed: {
    progress() {
      const total = this.tasks.length
      const done = this.tasks.filter((t) => !t.pending).length

      return Math.round((done / total) * 100) || 0
    },
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    addTask(task) {
      const sameName = (t) => t.name === task.name
      const reallyNew = this.tasks.filter(sameName).length == 0
      reallyNew &&
        this.tasks.push({
          name: task.name,
          pending: task.pending || true,
        })
    },
    deleteTask(i) {
      this.tasks.splice(i, 1)
    },
    toggleTaskState(i) {
      this.tasks[i].pending = !this.tasks[i].pending
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
  },
  mounted() {
    const json = localStorage.getItem('tasks')
    this.tasks = JSON.parse(json) || []
  },
}
</script>

<style>
body {
  font-family: 'Lato', sans-serif;
  background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
  color: #fff;
  scrollbar-base-color: yellowgreen;
}
::-webkit-scrollbar {
  width: 15px;
}

::-webkit-scrollbar-thumb {
  background: #0a8f08;
  border-radius: 8px;
}

::-webkit-scrollbar-thumb:hover {
  background: #00b32d;
}

#app {
  display: grid;
  place-content: center;
  min-height: 100vh;
}
.glassmorphism {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  backdrop-filter: blur(8px);
  border-radius: 8px;
  padding: 20px;
  box-sizing: border-box;
  background-color: rgba(255, 255, 255, 0.116);
}

#app::after {
  content: '';
  display: block;
  width: 100%;
  min-height: 100vh;
  position: fixed;
  top: 0;
  margin: 0;
  background-image: url('../public/img/background.png');
  background-color: rgba(0, 0, 0, 0.2);
  background-position: center;
  background-repeat: no-repeat;
  background-size: 100%;
  z-index: -1;
  opacity: 0.4;
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}

@media screen and (min-width: 700px) {
  #app::after {
    background-size: 800px;
  }
}
</style>
