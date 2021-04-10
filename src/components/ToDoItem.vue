<template>
  <div class="ToDoItem">
    <p class="ToDoItemText">{{ item.id }}. {{ item.text }}</p>
    <button class="ToDoItemDelete" @click="handleDeleteItem(item.id)">-</button>
  </div>
</template>

<script>
export default {
  name: 'ToDoItem',
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  setup(props, { emit }) {
    // ? 点击删除按钮，传递 id.
    const handleDeleteItem = id => {
      emit('delete', id);
    };

    return {
      handleDeleteItem
    };
  }
};
</script>

<style>
.ToDoItem {
  display: flex;
  justify-content: center;
  align-items: center;
}
.ToDoItemText {
  width: 90%;
  margin-right: 10px;
  padding: 12px;
  text-align: center;
  background-color: #fff;
  border-radius: 4px;
  border-color: rgba(0, 0, 0, 0.5);
  box-shadow: 5px 5px 13px #d9d9d9, -5px -5px 13px #fff;
}
.ToDoItemDelete {
  width: 50px;
  height: 50px;
  font-size: 2rem;
  color: red;
  background-image: linear-gradient(145deg, #e6e6e6, #fff);
  border-radius: 8px;
  border-color: transparent;
  box-shadow: 5px 5px 10px #ccc, -5px -5px 10px #fff;
  cursor: pointer;
}
.ToDoItemDelete:hover {
  box-shadow: 5px 5px 10px #ccc, -5px -5px 10px #fff;
}
.ToDoItemDelete:focus {
  border: none;
  outline: none;
}
</style>
