<template>
  <div class="home">
    <h1>{{ headline }}</h1>

    <div>
      <h4> Number of Tasks Remaining: {{ incompleteCounter() }}</h4>
    </div>

    <div>
      Search: <input v-model="taskFilter" list="texts">
      <datalist id="texts">
        <option v-for="task in tasks">{{ task.text }}</option>
      </datalist>
    </div>

    <div>
      <button @click="setSortAttribute('text')">Sort by Task</button>
    </div>

    <transition-group name="fade">
    <div v-for="task in orderBy(filterBy(tasks, textFilter, 'text'), sortAttribute, sortOrder)" v-bind:key="task.text">
      <h2>{{ task.text }}</h2>
    </div>
    </transition-group>
  </div>
</template>

<style>
  .fade-enter-active, .fade-leave-active {
    transition: opacity 10s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }

  /* Vue.js slide-right */
  .slide-right-enter-active {
    transition: all 1s ease;
  }
  .slide-right-leave-active {
    transition: all 1s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-right-enter, .slide-right-leave-to {
    transform: translateX(10px);
    opacity: 0;
  }

  /* Vue.js slide-left */
  .slide-left-enter-active {
    transition: all .3s ease;
  }
  .slide-left-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-left-enter, .slide-left-leave-to {
    transform: translateX(-10px);
    opacity: 0;
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
      taskFilter: "",
      sortAttribute: "text",
      sortOrder: 1
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
    },
    setSortAttribute: function(inputAttribute) {
      if (this.sortAttribute === inputAttribute) {
        this.sortOrder = this.sortOrder * -1;
      } else {
        this.sortOrder = 1;
      }
      this.sortAttribute = inputAttribute;
    }
  },
  computed: {}
};
</script>
