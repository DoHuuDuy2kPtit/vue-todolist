<template>
  <div class="App">
    <h1 class="App__title">TO DO LIST</h1>
    <div class="App__content">
      <BaseColumn
        v-for="(column, columnIndex) in columns"
        :key="column.id"
        :column="column"
        @handle-add-new-task="handleToggleModal"
      >
        <template>
          <draggable :list="column.tasks" group="task" @change="saveToLocal">
            <BaseTask
              v-for="(element, taskIndex) in column.tasks"
              :key="element.content"
              :task="element"
              :is-editing="element.id === editedTaskId"
              :column-index="columnIndex"
              :task-index="taskIndex"
              @handle-edit="handleEdit"
              @handle-cancel-edit="handleCancelEdit"
              @handle-change-task-content="handleChangeTaskContent"
              @handle-choose-edit-task="handleChooseEditTask"
              @handle-delete-task="handleDeleteTask(columnIndex, taskIndex)"
            ></BaseTask>
          </draggable>
        </template>
      </BaseColumn>
    </div>
    <div v-if="displayModal">
      <add-modal-task
        :taskContent="taskContent"
        :selectedColumn="selectedColumn"
        @handle-change-selected-column="handleChangeSelectedColumn"
        @handle-change-task-content="handleChangeTaskContent"
        @handle-add-new-task="handleAddNewTask"
        @handle-toggle-modal="handleToggleModal"
      />
    </div>
  </div>
</template>
<script>
import draggable from "vuedraggable";
import Vue from "vue";

import BaseTask from "./components/BaseTask.vue";
import BaseColumn from "./components/BaseColumn.vue";
import AddModalTask from "./components/AddModalTask.vue";

export default {
  name: "two-lists",
  display: "Two Lists",
  order: 1,
  components: {
    draggable,
    BaseColumn,
    BaseTask,
    AddModalTask,
  },
  data() {
    return {
      displayModal: false,
      editingColumnIndex: "",
      taskContent: "",
      editingTaskIndex: null,
      editedTaskId: null,
      selectedColumn: "",
      columns: [
        { id: "td", title: "TO DO", tasks: [] },
        { id: "ip", title: "IN PROGRESS", tasks: [] },
        { id: "de", title: "DONE", tasks: [] },
      ],
    };
  },
  mounted() {
    const columns = localStorage.getItem("columns");
    if (columns) {
      this.columns = JSON.parse(columns);
    }
  },
  methods: {
    saveToLocal() {
      localStorage.setItem("columns", JSON.stringify(this.columns));
    },
    handleToggleModal(choosenColumn = "") {
      this.displayModal = !this.displayModal;
      this.editingColumnIndex = choosenColumn;
    },
    handleChangeTaskContent(event) {
      this.taskContent = event.target.value;
    },
    handleChangeEditingColumnIndex(editingColumnIndex) {
      this.editingColumnIndex = editingColumnIndex;
    },
    handleAddNewTask(selected) {
      console.log(selected);
      if (this.taskContent.trim() === "") {
        Vue.$toast.warning("Please enter your task", { position: "top-right" });
      } else {
        const newTask = {
          id: new Date().getTime(),
          content: this.taskContent,
          time: new Date().toLocaleString(),
        };
        const columnIndex = this.columns.findIndex(
          (column) => column.id === selected
        );
        this.columns[columnIndex].tasks.push(newTask);
        this.editingColumnIndex = "";
        this.taskContent = "";
        this.editedTaskId = null;
        this.editingTaskIndex = null;
        this.displayModal = false;
        localStorage.setItem("columns", JSON.stringify(this.columns));
      }
    },
    handleDeleteTask(columnIndex, taskIndex) {
      const result = window.confirm("Are your sure to delete this task?");
      if (result) {
        this.columns[columnIndex].tasks.splice(taskIndex, 1);
        localStorage.setItem("columns", JSON.stringify(this.columns));
        Vue.$toast.success("Delete task success", { position: "top-right" });
      }
    },
    handleChooseEditTask(columnIndex, taskIndex, taskId) {
      this.editingColumnIndex = columnIndex;
      this.editingTaskIndex = taskIndex;
      this.editedTaskId = taskId;
    },
    handleChangeSelectedColumn(selectedColumn) {
      this.selectedColumn = selectedColumn;
    },
    handleEdit() {
      this.columns[this.editingColumnIndex].tasks[this.editingTaskIndex].content = this.taskContent;
      this.editingColumnIndex = "";
      this.taskContent = "";
      this.editedTaskId = null;
      this.editingTaskIndex = null;
      localStorage.setItem("columns", JSON.stringify(this.columns));
    },
    handleCancelEdit() {
      this.editingColumnIndex = "";
      this.taskContent = "";
      this.editedTaskId = null;
      this.editingTaskIndex = null;
    },
  },
};
</script>

<style lang="scss">
tml,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
.App {
  font-family: "Fira Sans", sans-serif;
  width: 1170px;
  margin: 0 auto;
  margin: 30px auto 0;
  overflow: auto;
  padding: 0 50px;
  &__title {
    font-size: 25px;
    text-align: center;
    margin-bottom: 24px;
    font-weight: bold;
    color: #5680f9;
  }
  &__content {
    display: flex;
    justify-content: space-between;
  }
}
</style>