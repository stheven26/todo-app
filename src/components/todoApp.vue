<template>
  <div class="container">
    <h2 class="text-center mt-5">Welcome to ToDo App</h2>
  </div>

  <!-- input -->
  <div class="d-flex mt-5 px-5">
    <input
      v-model="task"
      type="text"
      placeholder="Enter Task"
      class="form-control"
    />
    <button @click="submitTask" class="btn btn-warning rounded-0 mx-2">
      SUBMIT
    </button>
  </div>

  <!-- table -->
  <table class="table table-stripped table-bordered mt-5" style="width: 92.5%">
    <thead>
      <tr>
        <th scope="col" class="text-center" style="width: 250px">Task</th>
        <th scope="col" class="text-center" style="width: 250px">Status</th>
        <th scope="col" class="text-center" style="width: 120px">#</th>
        <th scope="col" class="text-center" style="width: 120px">#</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <th class="text-center">
          <span :class="{ finished: task.status === 'finished' }">{{
            task.name
          }}</span>
        </th>
        <td class="text-center" style="width: 120px">
          <span
            class="pointer"
            @click="changeStatus(index)"
            :class="{
              'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'in-progress',
              'text-success': task.status === 'finished',
            }"
            >{{ firstCharUpper(task.status) }}</span
          >
        </td>
        <td>
          <div class="text-center" @click="updateTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td>
          <div class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "todoApp",
  data() {
    return {
      task: "",
      updatedTask: null,
      availableStatus: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          name: "Learn Vue JS",
          status: "in-progress",
        },
        {
          name: "Learn English",
          status: "to-do",
        },
      ],
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.updateTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.updateTask].name = this.task;
        this.updateTask = null;
      }
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    updateTask(index) {
      this.task = this.tasks[index].name;
      this.updateTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatus[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style>
.pointer {
  cursor: pointer;
}
.table {
  margin-left: auto;
  margin-right: auto;
}
.finished {
  text-decoration: line-through;
}
</style>
