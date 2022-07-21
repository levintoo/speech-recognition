<script setup>
import { ref, onMounted } from 'vue'

const count = ref(0)
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])

onMounted(() => {
  alert("mounted");
});
</script>

<template>
  <div>
    <h1 @click="count++">Hello from vue {{ count }}</h1>
    <li v-for="item in items">
      {{ item.message }}
    </li>
  </div>
</template>

<style scoped>
h1 {
  cursor: pointer;
}
</style>
