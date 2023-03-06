<script setup lang="ts">
import Greet from './components/Greet.vue'

</script>

<template>
  <div class="container">
    <div>
      <div>{{obj}} :: {{simple}}::{{myHot}}</div>
      <button @click="addBtn">增加一层</button>
      <button @click="addWraper">增加二层</button>
      <button @click="editSim">改变字符串</button>
      <button @click="editArray">改变数组</button>
      <input type="text">
    </div>
  </div>
</template>

<style scoped>
.logo.vite:hover {
  filter: drop-shadow(0 0 2em #747bff);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #249b73);
}
</style>
