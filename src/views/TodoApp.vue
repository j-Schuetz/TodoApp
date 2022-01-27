<template>
  <div class="h-screen bg-gradient-to-b from-white to-red-400">
    <div class="text-center">
      <h1 class="text-5xl p-6 text-white font-bold bg-red-900 w-full">
        Vue Todo List
      </h1>
      <input
        v-model="task"
        class="border-2 pr-52 flex-auto hover:shadow-lg"
        type="text"
        placeholder="Enter task here:"
      />
      <button
        @click="addTask()"
        class="bg-red-800 hover:bg-red-400 hover:shadow-sm text-white font-bold py-1 px-4 m-2 rounded-sm"
      >
        Add
      </button>
    </div>
    <div class="flex justify-center w-full">
      <div class="flex flex-col bg-gray-800 overflow-auto">
        <div class="border-b border-gray-800 shadow">
          <table class="divide-y divide-gray-300 w-full">
            <thead class="bg-red-800">
              <tr>
                <th class="px-16 py-2 text-xs text-white">Task</th>
                <th class="px-16 py-2 text-xs text-white">Status</th>
                <th class="px-16 py-2 text-xs text-white">#</th>
                <th class="px-16 py-2 text-xs text-white">#</th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-300">
              <tr
                v-for="(task, index) in tasks"
                :key="index"
                class="whitespace-nowrap"
              >
                <td class="p-4 text-md text-gray-500 text-center">
                  {{ task.name }}
                </td>
                <td class="p-4 text-center">
                  <div
                    @click="changeStatus(index)"
                    class="text-base text-gray-900 cursor-pointer capitalize"
                  >
                    {{ task.status }}
                  </div>
                </td>

                <td class="px-4 py-4 text-center">
                  <button
                    class="rounded-full hover:shadow-2xl hover:bg-gray-100 p-2"
                    href="#"
                    @click="editTasks(index)"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-6 h-6 text-blue-400"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                      />
                    </svg>
                  </button>
                </td>
                <td class="px-4 py-4 text-center">
                  <button
                    class="rounded-full hover:shadow-2xl hover:bg-gray-100 p-2"
                    href="#"
                    @click="deleteTask(index)"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-6 h-6 text-red-400"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                      />
                    </svg>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <footer>
      <div
        class="text-white text-lg bg-red-800 text-center fixed bottom-0 w-full h-16"
      >
        <p>J-Schuetz &#128013;</p>
      </div>
    </footer>
  </div>
</template>

<script lang="ts">
export default {
  name: "TodoApp",

  data() {
    return {
      editTask: null as null | number,
      availableStatus: ["to-do", "active", "done"],
      tasks: [],
      task: "",
    };
  },
  methods: {
    addTask(): void {
      if (this.task.length === 0) return;

      if (this.editTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editTask].name = this.task;
        this.editTask = null;
      }
      this.saveTasks();
      this.task = "";
    },
    deleteTask(index: number) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    editTasks(index: number) {
      this.task = this.tasks[index].name;
      this.editTask = index;
    },
    changeStatus(index: number) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > this.availableStatus.length - 1) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
      this.saveTasks();
    },
    saveTasks() {
      let parsedTasks = JSON.stringify(this.tasks);
      localStorage.setItem("tasks", parsedTasks);
    },
  },
  mounted() {
    if (localStorage.getItem("tasks")) {
      try {
        this.tasks = JSON.parse(localStorage.getItem("tasks") || "[]");
      } catch (e) {
        localStorage.removeItem("tasks");
      }
    }
  },
};
</script>
