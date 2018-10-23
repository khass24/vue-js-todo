<template>
  <div class="home">
    <h1>{{ headline }}</h1>

    <div>
      <h4> Number of Tasks Remaining: {{ incompleteCounter() }}</h4>
    </div>

    <div>
      <input v-model="taskFilter">
    </div>

    <div v-for="task in filterBy(tasks, taskFilter, 'text')">
      <h2 @click="toggleBio(task)">{{ task.text }}</h2>
      <div v-if="task.completed">
        <h3>{{ task.text }}</h3>
        <button @click="deleteTask(task)">Delete</button>
      </div>
    </div>


    <!-- <div>
      Task: <input v-model="newTask.text">
      <button v-on:click="addTask()">Add Task</button>
    </div>

    <div v-for="task in tasks">
      <h3 @click="toggleComplete(task)" v-bind:class="{strike: task.completed}">{{ task.text}}</h3>
    </div> -->
  
<!--       <button @click="deleteTasks()">Delete Completed Tasks!</button>
 --> </div>
</template>

<style>

.strike {
  text-decoration: line-through;
}

</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      headline: "Your Daily Tasks",

      tasks: [],
      newTask: {text: "", completed: false},
      taskFilter: ""
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/tasks")
    .then(function(response) {
      this.tasks = response.data;
    }.bind(this));
  },
  methods: {
    addTask: function() {
      var params = {
                    text: this.newTask.text
                    };

      axios
      .post("http://localhost:3000/api/tasks", params)
      .then(function(response) {
        this.tasks.push(response.data);
      }.bind(this));

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
