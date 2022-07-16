<template>
  <div class="bg-white rounded shadow p-6 m-4 w-full">
    <div class="mb-4">
      <h1 class="text-grey-darkest text-3xl">Todo List</h1>
      <div class="flex mt-4">
        <input
          v-model="taskName"
          type="text"
          class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
          placeholder="Add Todo"
        />
        <button
          @click="submitTask"
          class="flex-no-shrink p-2 border-2 border-teal-500 rounded text-teal-400 border-teal hover:text-white hover:bg-teal-400"
        >
          Submit
        </button>
        <button
          class="ml-2 flex-no-shrink p-2 border-2 border-teal-500 rounded text-teal-400 border-teal hover:text-white hover:bg-teal-400"
          @click="saveTask"
        >
          Save
        </button>
        <button
          @click="clearAll()"
          class="ml-2 flex-no-shrink p-2 border-2 border-teal-500 rounded text-teal-400 border-teal hover:text-white hover:bg-teal-400"
        >
          ClearAll
        </button>
      </div>
    </div>
    <div class="table w-full">
      <div v-for="(task, index) in tasks" :key="task.id">
        <ol class="grid grid-cols-4 gap-4 w-full">
          <div v-if="!task.editing" @dblclick="editTask(task)">
            <li
              class="text-2xl w-96 text-grey-darkest pt-2 mt-5 mb-5"
              :class="task.status === 'true' ? 'line-through' : ''"
            >
              {{ task.name }}
            </li>
          </div>
          <input
            v-model="task.name"
            v-else
            type="text"
            @blur="doneEdit(task)"
            @keyup.enter="doneEdit(task)"
          />
          <div class="text-left p-2 ml-40 mt-2">
            <input
              class="mr-10"
              type="radio"
              value="true"
              v-model="task.status"
            />
            <label>Done</label><br />
            <input
              class="mr-10"
              type="radio"
              value="false"
              v-model="task.status"
            />
            <label>Not Done</label><br />
          </div>

          <button
            @click="deleteTask(index)"
            class="w-48 ml-20 mb-2 mt-2 border-2 rounded border-red-500 text-red-500 hover:bg-red-700 hover:text-white hover:border-none"
          >
            Remove
          </button>
          <button
            @click="editTask(task)"
            class="w-48 ml-20 mb-2 mt-2 border-2 rounded border-red-500 text-red-500 hover:bg-red-700 hover:text-white hover:border-none"
          >
            Edit
          </button>
        </ol>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "",
  data() {
    return {
      taskName: "",
      taskId: 0,
      tasks: [],
      editing: false,
    };
  },
  mounted() {
    const getItem = localStorage.getItem("tasks");
    if (getItem) {
      try {
        this.tasks = JSON.parse(getItem);
      } catch {
        localStorage.removeItem("tasks");
      }
    }
  },
  methods: {
    submitTask() {
      if (this.taskName.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.taskName;
        this.editedTask = null;
      } else {
        this.tasks.push({
          id: this.taskId,
          name: this.taskName,
          status: false,
        });
      }
      this.taskId++;
      this.taskName = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(task) {
      task.editing = true;
    },
    doneEdit(task) {
      task.editing = false;
    },
    saveTask() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsed);
    },
    clearAll() {
      this.tasks = [];
    },
  },
};
</script>
