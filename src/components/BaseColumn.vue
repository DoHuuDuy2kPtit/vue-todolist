<template>
  <div class="Column">
    <div class="Column__header">
      <h2 class="Column__title">
        <span class="Column__item-count">{{ column.tasks.length }}</span>
        <span class="Column__text">{{ column.title }}</span>
      </h2>
      <p class="Column__btn" @click="$emit('handle-add-new-task', column.id)" >
        <i class="fas fa-plus"></i> New task
      </p>
    </div>
    <div class="Column__content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: ['handleAddNewTask', 'column'],
}
</script>

<style lang="scss">
.Column {
    width: 320px;
    border: 1px solid #e1e4e8;
    border-radius: 5px;
    background-color: #F7F8FE;
    padding: 10px 20px;
    height: fit-content;
    &__header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 24px;
    }
    &__item-count {
        line-height: 30px;
        width: 30px;
        background-color: #5680f9;
        border-radius: 100%;
        display: inline-block;
        text-align: center;
        margin-right: 7px;
        color: #fff;
    }
    &__text {
        font-weight: bold;
        font-weight: 18px;
        letter-spacing: 1px;
    }
    &__btn {
        background-color: #5680f9;
        color: #fff;
        padding: 5px 7px;
        border-radius: 5px;
        cursor: pointer;
    }
}
</style>