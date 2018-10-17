<template>
  <div class="home">
    <h1>{{ headline }}</h1>

    <div>
      <h4> Number of Tasks Remaining: {{ incompleteCounter() }}</h4>
    </div>


    <div>
      Task: <input v-model="newTask.text">
      <button v-on:click="addTask()">Add Task</button>
    </div>

    <div v-for="task in tasks">
      <h3 @click="toggleComplete(task)" v-bind:class="{strike: task.completed}">{{ task.text}}</h3>
    </div>
  
      <button @click="deleteTasks()">Delete Completed Tasks!</button>
  </div>
</template>

<style>

.strike {
  text-decoration: line-through;
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
      completed: false
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
    },
    toggleComplete: function(inputTask) {
      inputTask.completed = !inputTask.completed;
    },
    incompleteCounter: function() {
      var count = 0;
      this.tasks.forEach(function(task) {
        if (!task.completed) {
          count ++;
        }
      });
      return count;
    },
    deleteTasks: function() {
      var incompleteTasks = [];
      for(var i = 0; i < this.tasks.length; i++) {
        var task = this.tasks[i];

        if (!task.completed) {
          incompleteTasks.push(task);
        }
      }
      this.tasks = incompleteTasks;
    }
  },
  computed: {}
};
</script>
