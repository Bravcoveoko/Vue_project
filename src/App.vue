<template>
  <div class="container">
    <div class="row" style="margin-top: 50px">

      <!--      IDEA      -->
      <div class="col  bg-light border rounded">
        <h6>Idea</h6>
        <draggable class="draggable-list" :list="tasks.ideas" group="tasks">

          <task v-bind:tasks_list="tasks.ideas" v-bind:color_class="idea_color" v-bind:task_name="idea" v-on:dblclick="textable" v-on:click="add_item"/>

        </draggable>
      </div>
      <!--      TODOS      -->
      <div class="col  bg-light border rounded">
        <h6>Todo</h6>
        <draggable class="draggable-list" :list="tasks.todos" group="tasks" >

          <task v-bind:tasks_list="tasks.todos" v-bind:color_class="todo_color" v-bind:task_name="todo" v-on:dblclick="textable" v-on:click="add_item"/>

        </draggable>
      </div>
      <!--      IN PROGRESS      -->
      <div class="col  bg-light border rounded">
        <h6>In Progress <span style="color: yellow"></span></h6>
        <draggable class="draggable-list" :list="tasks.inProgress" group="tasks">

          <task v-bind:tasks_list="tasks.inProgress" v-bind:color_class="inprogress" v-bind:task_name="progress" v-on:dblclick="textable" v-on:click="add_item"/>

        </draggable>
      </div>
      <!--      READY TO GO      -->
      <div class="col  bg-light border rounded">
        <h6>Ready to go </h6>
        <draggable class="draggable-list" :list="tasks.completed" group="tasks">

          <task v-bind:tasks_list="tasks.completed" v-bind:color_class="done" v-bind:task_name="ready" v-on:dblclick="textable" v-on:click="add_item"/>

        </draggable>
      </div>
      <!--      TRASH      -->
      <div class="col  bg-light border rounded">
        <h6>Trash </h6>
        <draggable class="draggable-list trash" group="tasks">
          <div>
          </div>
        </draggable>
      </div>

    </div>
  </div>
</template>
<script>
import { VueDraggableNext } from 'vue-draggable-next'
import Task from './components/Task'
export default {
  components: {
    draggable: VueDraggableNext,
    task: Task,
  },
  data() {
    return {
      tasks: {
        ideas: [{'id': (new Date()).getTime(), 'textable': false, 'content': "fdfdff"}, {'id': (new Date()).getTime(), 'textable': false, 'content': "fdfdff"}],
        todos: [{'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}, {'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}],
        inProgress: [{'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}],
        completed: [{'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}, {'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}, {'id': (new Date()).getTime(), 'textable': false, 'content': "Dockerize App"}],
      },
      idea_color: 'idea_color',
      inprogress: 'inprogress',
      todo_color: 'todo_color',
      done: 'done',
      idea: 'idea',
      todo: 'todo',
      progress: 'progress',
      ready: 'ready',
      newContent: '',
      current_input: null,
    };
  },

  methods: {
    add_item: function (column) {
      switch (column) {
        case 'idea':
          this.tasks.ideas.push({'id': (new Date()).getTime(), 'textable': false, 'content': "New task"});
          break;
        case 'todo':
          this.tasks.todos.push({'id': (new Date()).getTime(), 'textable': false, 'content': "New task"});
          break;
        case 'progress':
          this.tasks.inProgress.push({'id': (new Date()).getTime(), 'textable': false, 'content': "New task"});
          break;
        default:
          this.tasks.completed.push({'id': (new Date()).getTime(), 'textable': false, 'content': "New task"});
      }
    },

    textable: function (idea) {
      if (!this.current_input) {
        this.current_input = idea;
      } else {
        this.current_input.textable = false;
        this.current_input = idea;
      }
      idea.textable = true;
    },

    onEnter: function (idea) {
      idea.content = this.newContent === '' ? idea.content : this.newContent;
      idea.textable = false;
      this.current_input = null;
    },

    changeText: function (event) {
      this.newContent = event.target.value;
    },
  }
};
</script>

<style scoped>

.btn {
  background-color: black;
  font-weight: bolder;
  color: white;
}

p {
  color: white;
}
h6 {
  font-weight: 700;
}
.col {
  height: 90vh;
  margin-right: 20px;
  overflow: auto;
}

.col h6 {
  margin-top: 10px;
  text-align: center;
}
.draggable-list {
  min-height: 10vh;
}
.draggable-list > div {
  cursor: pointer;
}

.done {
  background-color: #349D49;
}

.inprogress {
  background-color: #E4D4AF;
}

.idea_color {
  background-color: #6D66CC;
}

.todo_color {
  background-color: #747E2A;
}

.rounded {
  word-wrap: break-word;
}

.trash {
  height: 400px;
}

</style>