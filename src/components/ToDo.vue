<template>
  <div class="ToDo">
    <img class="Logo" :src="logo" alt="Vue logo" />
    <h1 class="ToDoHeader">Vue 3.0 Todo with Vite</h1>
    <section class="ToDoWrapper">
      <section class="ToDoContent">
        <to-do-item v-for="item in todoList" :key="item.id" :item="item" @delete="deleteItem"></to-do-item>
      </section>
      <section class="ToDoInput">
        <input type="text" placeholder="I need to do..." v-model="todoMsg" @keyup.enter="handleCreateNewToDoItem" />
        <button class="ToDoAdd" @click="handleCreateNewToDoItem">+</button>
      </section>
      <section class="ToDoErrorWrapper"><p v-if="showError">Todo message is required!</p></section>
    </section>
  </div>
</template>

<script>
import { ref } from 'vue';
import ToDoItem from './ToDoItem.vue';
import Logo from '../assets/logo.png';

export default {
  name: 'ToDo',
  components: { ToDoItem },
  setup() {
    /**
     * * logo 图标。
     * * showError 是否显示为空提示。
     * * todoMsg 输入的 todo 信息。
     */
    const logo = Logo;
    const todoList = ref([
      { id: 1, text: 'Eat breakfast' },
      { id: 2, text: 'Clean the house' }
    ]);
    const showError = ref(false);
    const todoMsg = ref('');

    // ? 展示空错误提示。
    const displayError = () => {
      showError.value = true;
      const clearTimer = setTimeout(() => (showError.value = false), 3000);
      return () => clearTimeout(clearTimer);
    };
    // ? 删除对应 item.
    const deleteItem = id => {
      todoList.value = todoList.value.filter(item => item.id !== id);
    };
    // ? 生成 todo id.
    const generateId = () => {
      if (todoList.value && todoList.value.length) return Math.max(...todoList.value.map(item => item.id)) + 1;
      return 1;
    };
    // ? 点击 "+" 生成新的 todoItem.
    const handleCreateNewToDoItem = () => {
      if (!todoMsg.value) {
        displayError();
        return;
      }
      const newId = generateId();
      todoList.value.push({ id: newId, text: todoMsg.value });
      console.info(todoList.value);
      todoMsg.value = '';
    };

    return {
      logo,
      todoList,
      showError,
      todoMsg,
      deleteItem,
      handleCreateNewToDoItem
    };
  }
};
</script>

<style>
.ToDo {
  width: 80vw;
  max-width: 768px;
  max-height: 75vh;
  text-align: center;
  background-color: #fff;
  border-radius: 8px;
  overflow-y: scroll;
}
.Logo {
  position: relative;
  top: 50px;
  width: 50px;
}
@media (prefers-reduced-motion: no-preference) {
  .Logo {
    animation: App-logo-spin infinite 1.5s linear;
  }
}
.ToDoHeader {
  margin: 70px auto 30px;
  color: #333;
}
.ToDoWrapper {
  width: 80%;
  margin: 0 auto;
}
.ToDoInput {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 10px;
}
.ToDoAdd {
  width: 50px;
  height: 50px;
  font-size: 2rem;
  line-height: 48px;
  color: skyblue;
  background-image: linear-gradient(145deg, #e6e6e6, #fff);
  border-radius: 8px;
  border-color: transparent;
  box-shadow: 5px 5px 10px #ccc, -5px -5px 10px #fff;
  cursor: pointer;
}
.ToDoAdd:hover {
  box-shadow: 5px 5px 15px #ccc, -5px -5px 15px #fff;
}
.ToDoAdd:focus {
  border: none;
  outline: none;
}
.ToDoErrorWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100px;
  font-size: 1rem;
  color: red;
}
@keyframes App-logo-spin {
  from {
    transform: scale(0.8);
  }
  to {
    transform: scale(1);
  }
}
</style>
