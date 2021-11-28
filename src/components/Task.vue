<template>
  <div v-for="idea in tasks_list" :key="idea.id" v-on:dblclick="callbackTextable(idea)">
    <div class="mt-3 p-2 shadow border rounded isDraggable" :class="color_class" v-if="!idea.textable">
      <p>{{ idea.content }}</p>
    </div>

    <div class="mt-3 p-2 shadow border rounded" :class="color_class" v-else>
      <div class="input-group mb-3">
        <textarea type="text" class="form-control" aria-describedby="basic-addon1" :value="idea.content" @keyup.enter="onEnter(idea)" @input="changeText($event)"></textarea>
      </div>
    </div>
  </div>



  <div class="" style="text-align: center; margin-top: 15px ">
    <button type="button" class="btn" v-on:click="add_item();" >Add task</button>
  </div>
</template>

<script>
export default {
  name: "task",
  props: ['tasks_list', 'color_class', 'task_name'],
  emits: ['dblclick', 'click'],

  data() {
    return {
      newContent: '',
      current_input: null,
    }
  },

  methods: {
    callbackTextable: function (idea) {
      this.$emit('dblclick', idea);
    },

    onEnter: function (idea) {
      idea.content = this.newContent === '' ? idea.content : this.newContent;
      idea.textable = false;
      this.current_input = null;
    },

    changeText: function (event) {
      this.newContent = event.target.value;
    },

    add_item: function () {
      this.$emit('click', this.task_name);
    },
  }

}
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