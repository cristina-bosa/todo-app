<template>
  <h1 class="text-4xl font-bold text-center my-5">#todo</h1>
  <div class="grid grid-cols-2 gap-6">
    <div class="col-span-2 px-9 py-9">
      <nav>
        <button
          @click="changeOption('all')"
          :class="
            buttonPress == 'all'
              ? 'text-blue-500 p-5 border-b-2 border-blue-600'
              : 'border-none p-5'
          "
        >
          All
        </button>
        <button
          @click="changeOption('active')"
          :class="
            buttonPress == 'active'
              ? 'text-blue-500 p-5 border-b-2 border-blue-600'
              : 'border-none p-5'
          "
        >
          Active
        </button>
        <button
          @click="changeOption('completed')"
          :class="
            buttonPress == 'completed'
              ? 'text-blue-500 p-5 border-b-2 border-blue-600'
              : 'border-none p-5'
          "
        >
          Completed
        </button>
      </nav>
    </div>

    <div class="col-span-2 px-9">
      <div v-if="buttonPress != 'completed'" class="grid grid-cols-2 gap-0">
        <div>
          <input
            type="text"
            v-model="newTask"
            class="border border-gray-400 rounded-lg h-50 p-2"
            placeholder="Add details..."
            @keypress.enter="addNewTask()"
          />
        </div>
        <div>
          <button
            class="p-4 bg-blue-500 text-white rounded-md"
            @click="addNewTask()"
          >
            Add
          </button>
        </div>
      </div>
    </div>

    <div>
      <template v-for="task in filteredTask" :key="task.id">
        <Task
          :task="task"
          :buttonPress="buttonPress"
          @deleteTask="deleteTask(task.id)"
          @isActive="isActive(task.id)"
          @isCompleted="isCompleted(task.id)"
        />
      </template>

      <button
        v-if="buttonPress == 'completed'"
        class="bg-red-600 p-2 text-white"
        @click="deleteAllTask()"
      >
        delete all
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import Task from "./components/Task.vue";

export default defineComponent({
  name: "App",
  components: { Task },
  setup() {
    const allTasks = ref([
      {
        id: 1,
        title: "delectus aut autem",
        completed: false,
        active: false,
      },
      {
        id: 2,
        title: "quis ut nam facilis et officia qui",
        completed: false,
        active: true,
      },
      {
        id: 3,
        title: "fugiat veniam minus",
        completed: false,
        active: true,
      },
      {
        id: 4,
        title: "et porro tempora",
        completed: false,
        active: true,
      },
      {
        id: 5,
        title:
          "laboriosam mollitia et enim quasi adipisci quia provident illum",
        completed: true,
        active: false,
      },
      {
        id: 6,
        title: "qui ullam ratione quibusdam voluptatem quia omnis",
        completed: true,
        active: true,
      },
    ]);

    const buttonPress = ref("all");
    const filteredTask = computed(() => {
      switch (buttonPress.value) {
        case "all":
          return allTasks.value;
        case "active":
          return allTasks.value.filter((task) => task.active == true);
        case "completed":
          return allTasks.value.filter((task) => task.completed == true);
      }
    });

    const changeOption = (option: string) => {
      buttonPress.value = option;
    };
    const isActive = (id: number) => {
      allTasks.value.find((task) => {
        if (task.id === id) task.active = !task.active;
      });
    };

    const isCompleted = (id: number) => {
      allTasks.value.find((task) => {
        if (task.id === id) {
          task.completed = !task.completed;
        }
      });
    };
    const newTask  = ref("");
    let nextId = allTasks.value.length + 1;
    const addNewTask = () => {
      console.log('a');
      allTasks.value.push({
        id: nextId,
        title: newTask.value,
        completed: false,
        active: false
      });
      nextId++;
      newTask.value = '';
    };

    const deleteAllTask = () => {
      allTasks.value = allTasks.value.filter((task) => task.completed != true);
      nextId = 1;
    };

    const deleteTask = (id: number) => {
      allTasks.value = allTasks.value.filter((task) => task.id != id);
    };

    return {
      allTasks,
      filteredTask,
      changeOption,
      addNewTask,
      newTask,
      deleteAllTask,
      buttonPress,
      deleteTask,
      isActive,
      isCompleted,
    };
  },
});
</script>
