<template>
  <div class="container">
    <h2 class="text-center mt-4">Các công việc cần làm</h2>

    <!-- noi nhap input -->
    <div class="d-flex mt-3">
      <input
        type="text"
        v-model="task"
        placeholder="Nhập công việc cần làm"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-success rounded-0">
        Submit
      </button>
    </div>

    <!-- Bang cong viec can lam -->
    <table class="table table-bordered mt-3">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col">Edit</th>
          <th scope="col">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td style="width: 150px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'to do',
                'text-warning': task.status === 'in process',
                'text-success': task.status === 'finished',
              }"
            >
              {{ task.status }}
            </span>
          </td>
          <td>
            <div @click="editTask(index)" class="d-flex justify-content-center">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div
              @click="deleteTask(index)"
              class="d-flex justify-content-center"
            >
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["to do", "in process", "finished"],

      tasks: [
        {
          name: "Day som 6h sang",
          status: "to do",
        },
        {
          name: "Tap gym vao 5h chieu",
          status: "in process",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) {
        newIndex = 0;
      }
      this.tasks[index].status = this.statuses[newIndex];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
