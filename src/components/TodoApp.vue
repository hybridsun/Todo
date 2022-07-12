<template>
  <div class="container" style="max-width: 600px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Vuejs Todo App</h2>

    <!-- Input -->
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="type your text here"
        class="w-100 form-control"
      />
      <button class="btn btn-danger rounded" @click="submitTask">
        SUBMIT
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task List</th>
          <th scope="col" style="width: 110px">Status</th>
          <th scope="col" class="text-center">Delete</th>
          <th scope="col" class="text-center">Edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'Completed' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-info': task.status === 'To-do',
                'text-danger': task.status === 'Priority',
                'text-warning': task.status === 'In-progress',
                'text-success': task.status === 'Completed',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld !",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["To-do", "In-progress", "Priority", "Completed"],
      tasks: [
        {
          name: "Write an article.",
          status: "In-progress",
        },
        {
          name: "Finish breakfast.",
          status: "To-do",
        },
        {
          name: "Create a Vuejs app for assignment",
          status: "Completed",
        },
        {
          name: "Mocha or chai",
          status:"Priority",
        },
      ],
    };
  },

  methods: {
    /**
     * Capitalize first character
     */
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    /**
     * Change status of task by index
     */
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>