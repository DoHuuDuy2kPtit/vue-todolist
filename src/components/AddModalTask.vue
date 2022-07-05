<template>
  <div class="AddNewModal">
    <div class="AddNewModal__backdrop"></div>
    <div class="AddNewModal__content">
      <h4 class="AddNewModal__title">CREATE NEW TASK</h4>
      <div class="AddNewModal__task-status">
        <span class="AddNewModal__radio">
          <input type="radio" value="td" v-model="selected" />
          <span>TODO</span>
        </span>
        <span class="AddNewModal__radio">
          <input type="radio" value="ip" v-model="selected" />
          <span>IN PROGRESS</span>
        </span>
        <span class="AddNewModal__radio">
          <input type="radio" value="de" v-model="selected" />
          <span>DONE</span>
        </span>
      </div>
      <div class="AddNewModal__task">
        <input
          class="AddNewModal__input"
          type="text"
          placeholder="Enter your task..."
          :value="taskContent"
          @input="$emit('handle-change-task-content', $event)"
        />
      </div>
      <div class="AddNewModal__action">
        <button
          class="AddNewModal__btn AddNewModal__btn--confirm"
          @click="$emit('handle-add-new-task', selected)"
        >
          Save
        </button>
        <button
          class="AddNewModal__btn AddNewModal__btn--cancel"
          @click="$emit('handle-toggle-modal')"
        >
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["taskContent", "selectedColumn"],
  data() {
    return {
      selected: this.$props.selectedColumn,
    };
  },
};
</script>

<style lang="scss">
.AddNewModal {
  position: fixed;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  left: 0;
  border: 1px solid #5680f9;
  &__backdrop {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(33, 33, 33, 0.3);
  }
  &__content {
    position: relative;
    z-index: 9;
    width: 350px;
    background-color: #fff;
    padding: 10px 15px;
    border-radius: 5px;
    box-sizing: border-box;
    margin-top: -10%;
  }
  &__title {
    font-weight: bold;
    letter-spacing: 0.5px;
    line-height: 30px;
    box-sizing: border-box;
    border-bottom: 1px solid #ccc;
    color: #5680f9;
  }
  &__task-status {
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  &__radio {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  &__task {
    margin: 7px 0 25px;
    box-sizing: border-box;
  }
  &__input {
    line-height: 30px;
    width: 100%;
    border-radius: 3px;
    border: 1px solid #ccc;
    text-indent: 7px;
  }
  &__action {
    text-align: right;
  }
  &__btn {
    border: 1px solid transparent;
    width: 70px;
    box-sizing: border-box;
    line-height: 25px;
    margin: 0 7px;
    border-radius: 3px;
    font-size: 15px;
    cursor: pointer;
    &--confirm {
      background-color: #5680f9;
      color: #fff;
      border-color: #5680f9;
    }
    &--cancel {
      background-color: #fff;
      border-color: #666a95;
      color: #666a95;
    }
  }
  &__btn:last-child {
    margin-right: 0;
  }
}
</style>