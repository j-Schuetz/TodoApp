<template>
  <div>
    <div class="text-center">
      <h1 class="text-5xl p-8 text-purple-600 font-bold">Vue Todo List</h1>
      <input
        v-model="task"
        class="border-2 pr-52 flex-auto"
        type="text"
        placeholder="Enter task here:"
      />
      <button
        @click="addTask()"
        class="bg-purple-500 hover:bg-purple-700 text-white font-bold py-1 px-4 m-2 rounded-sm"
      >
        Add
      </button>
    </div>
    <!-- Table -->
    <div class="container flex justify-center mx-auto w-full">
      <div class="flex flex-col">
        <div class="border-b border-gray-200 shadow">
          <table class="divide-y divide-gray-300">
            <thead class="bg-gray-50">
              <tr>
                <th class="px-6 py-2 text-xs text-gray-500">Task</th>
                <th class="px-6 py-2 text-xs text-gray-500">Status</th>
                <th class="px-6 py-2 text-xs text-gray-500">#</th>
                <th class="px-6 py-2 text-xs text-gray-500">#</th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-300">
              <tr
                v-for="(task, index) in tasks"
                :key="index"
                class="whitespace-nowrap"
              >
                <td class="px-6 py-4 text-sm text-gray-500">{{ task.name }}</td>
                <td class="px-6 py-4">
                  <div
                    @click="changeStatus(index)"
                    class="text-sm text-gray-900 cursor-pointer capitalize"
                  >
                    {{ task.status }}
                  </div>
                </td>

                <td class="px-6 py-4">
                  <a href="#" @click="editTasks(index)">
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
                  </a>
                </td>
                <td class="px-6 py-4">
                  <a href="#" @click="deleteTask(index)">
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
                  </a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "TodoApp",

  data() {
    return {
      editTask: null as null | number,
      availableStatus: ["to-do", "active", "done"],
      tasks: [
        {
          name: "moep people",
          status: "to-do",
        },
        {
          name: "Do something",
          status: "active",
        },
      ],
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
      this.task = "";
    },
    deleteTask(index: number) {
      this.tasks.splice(index, 1);
    },
    editTasks(index: number) {
      this.task = this.tasks[index].name;
      this.editTask = index;
    },
    changeStatus(index: number) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > this.availableStatus.length - 1) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    },
  },
};
</script>
