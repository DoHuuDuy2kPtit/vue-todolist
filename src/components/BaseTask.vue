<template>
  <div v-if="!isEditing" class="Task">
    <div class="Task__time">
      <i class="far fa-calendar-alt"></i> {{ task.time }}
    </div>
    <div class="Task__main">
      <div class="Task__content">{{ task.content }}</div>
      <div class="Task__action">
        <div
          class="Task__btn"
          @click="
            $emit('handle-choose-edit-task', 
              columnIndex,
              taskIndex,
              task.id,
            )
          "
        >
          <i class="far fa-edit"></i>
        </div>
        <div class="Task__btn" @click="$emit('handle-delete-task')">
          <i class="far fa-trash-alt"></i>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div class="Task__editing">
      <input
        type="text"
        class="Task__editor"
        :value="task.content"
        @input="$emit('handle-change-task-content', $event)"
      />
      <div class="Task__editing-action">
        <i class="fas fa-check" @click="$emit('handle-edit')"></i>
        <i class="fas fa-ban" @click="$emit('handle-cancel-edit')"></i>
      </div>
      <div
        class="Task__editing-bgr"
        @click="$emit('handle-cancel-edit')"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["task", "isEditing", "columnIndex", "taskIndex"],
};
</script>

<style lang="scss" scoped>
.Task {
  width: 100%;
  background-color: #fff;
  box-shadow: 0 3px 7px 0 rgba(110, 142, 247, 0.13);
  border: 1px solid #e1e2e8;
  border-radius: 3px;
  padding: 15px 10px;
  box-sizing: border-box;
  margin-bottom: 15px;
  &__editing {
    flex-grow: 2;
    word-wrap: break-word;
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-sizing: border-box;
  }
  &__editing-bgr {
    position: fixed;
    top: 0;
    left: 0;
    background-color: rgba(247, 248, 254, 0.5);
    width: 100vw;
    height: 100vh;
  }
  &__editor {
    width: 100%;
    margin-right: 5px;
    text-indent: 2px;
    font-size: 17px;
    box-sizing: border-box;
    z-index: 9;
  }
  &__editing-action {
    display: flex;
    z-index: 99;
    i {
      cursor: pointer;
      color: #666666;
      margin-left: 5px;
      &:first-child {
        color: #00df82;
      }
    }
  }
  &__time {
    font-size: 13px;
    color: #666666;
    margin-bottom: 10px;
    i {
      margin-right: 5px;
    }
  }
  &__main {
    display: flex;
  }
  &__content {
    flex-grow: 2;
    padding-right: 10px;
    word-wrap: break-word;
    font-size: 17px;
  }
  &__action {
    display: flex;
  }
  &__btn {
    margin: 0 5px;
    cursor: pointer;
    &:first-child {
      color: #5680f9;
    }
    &:nth-child(2) {
      color: #ff2f2f;
    }
  }
}
</style>