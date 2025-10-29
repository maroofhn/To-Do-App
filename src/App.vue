<template>
  <div class="textbox">
    <h1>{{ greeting }}</h1>
    <h3>What's Next?</h3>

    <div class="entertask">

      <!-- Takes Input From the user, User can Press Enter to Add Task-->
      <input
        type="text"
        class="inputtext"
        v-model="newTask"
        placeholder="Add a task"
        @keyup.enter="addTask"
      />

      <!--If User does not press enter, they can click + button-->
      <button class="inputbutton" @click="addTask">
        <img src="/src/assets/plus-symbol-button.png" alt="Add" class="plusimg" />
      </button>
    </div>

    <!--Checks if Tasks Length is 0, displays text-->
    <p class="notasktext" v-if="tasks.length === 0"><i>* cricket noises *</i></p>

    <!--Unordered List for TodoItems, operates loops to display data, also contains remove/toggle functionality-->
    <ul>
      <TodoItem
        v-for="(task, index) in tasks"
        :key="index"
        :task="task"
        @remove="removeTask(index)"
        @toggle="toggleComplete(index)"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'

// All Components for Program
export default {
  components: { TodoItem },

  // All Data to be declared
  data() {
    return {
      newTask: '',
      tasks: [],
      greeting: '',
    }
  },

  // All Methods for Program
  methods: {
    addTask() {
      if (this.newTask.trim() === '') return
      this.tasks.push({ text: this.newTask.trim(), completed: false })
      this.newTask = ''
    },

    removeTask(index) {
      this.tasks.splice(index, 1)
    },

    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed
    },

    // Updates Greeting accordingly to time of user.
    updateGreeting() {
      const hour = new Date().getHours()
      if (hour < 12) this.greeting = 'Good Morning.'
      else if (hour < 18) this.greeting = 'Good Afternoon.'
      else this.greeting = 'Good Evening.'
    },
  },

  // Saves data locally, boots up next time opened.
  created() {
    const saved = localStorage.getItem('tasks')
    if (saved) this.tasks = JSON.parse(saved)
  },

  updated() {
    localStorage.setItem('tasks', JSON.stringify(this.tasks))
  },

  mounted() {
    this.updateGreeting()
    setInterval(this.updateGreeting, 60000)
  },
}
</script>
