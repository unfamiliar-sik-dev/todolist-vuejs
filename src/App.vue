<!-- js  -->
<script setup>
import { ref } from 'vue';
/**
  todo item
  key: text, done
  item.text or item['text']
  {
    text: string
    done: boolean(true/false)
  }
*/

// ㄴㅇㄴㄴㅇ
const inputValue = ref('');
const todoList = ref([
  { text: 'vue', done: false },
  { text: 'react', done: false },
  { text: 'vercel', done: false },
]);

const handleClickButton = () => {
  todoList.value.push(inputValue.value);
};

const handleChange = (event) => {
  const nextValue = event.target.value;
  inputValue.value = nextValue;
};

const handleClickDeleteButton = (index) => {
  todoList.value.splice(index, 1);
};
</script>

<!-- html -->
<template>
  <p>{{ todoList }}</p>
  <h4>TODO LIST</h4>

  <input v-model="inputValue" />
  <!-- <input :value="inputValue" @change="handleChange" /> -->

  <button @click="handleClickButton">확인</button>

  <div class="todo-item" v-for="(item, index) in todoList">
    <input type="checkbox" v-model="item.done" />
    <span :class="{ 'done-item': item.done }">{{ item.text }}</span>
    <div>
      <button>수정</button>
      <button @click="handleClickDeleteButton(index)">삭제</button>
    </div>
  </div>
</template>

<!-- css -->
<style scoped>
.todo-item {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  border: 1px solid gray;
}

.done-item {
  text-decoration-line: line-through;
}
</style>
