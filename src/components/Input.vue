<script setup>
import { ref } from 'vue';

const text = ref('');
const emits = defineEmits(['onNewItem']);

const handleSubmit = () => {
  if (text.value !== '') {
    emits('onNewItem', text.value);
    text.value = '';
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" v-model="text">
    <button type="submit">Add item</button>
  </form>
</template>

<style scoped>
  form {
    display: flex;
  }

  input {
    flex: 1;
    border-radius: 8px;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    padding: 6px 12px;
    border: 1px solid #000;
  }

  button {
    border-radius: 8px;
    border: 1px solid #09f;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    padding: 6px 6px;
    background-color: #09f;
    color: white;
  }
</style>