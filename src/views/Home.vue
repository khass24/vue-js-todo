<template>
  <div class="home">
    <h1>{{ headline }}</h1>

    <div>
      Task: <input v-model="newTask.text">
      <button v-on:click="addTask()">Add Task</button>
    </div>

    <div v-for="task in tasks">
      <h3>{{ task.text}}</h3>
      <h3>Completed? {{ task.completed }}</h3>
    </div>
  </div>
</template>

<style>
body {
  background-color: gray;
}
</style>

<script>
export default {
  data: function() {
    return {
      headline: "Your Daily Tasks",

      tasks: [
      { 
      text: "Go to the gym",
      completed: true
      },
      { 
      text: "Walk the dog",
      completed: false
      },
      {
      text: "Take a bath",
      completed: false
      }
    ],
    newTask: {text: "", completed: false}
    };
  },
  created: function() {},
  methods: {
    addTask: function() {
      this.tasks.push(this.newTask);
      this.newTask = {text: "", completed: false};
    }
  },
  computed: {}
};
</script>
