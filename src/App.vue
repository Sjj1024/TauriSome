<script setup>
import { reactive, ref } from '@vue/reactivity'

// 将数据变为响应式的:支持多层签到
let obj = reactive({
  a: 1,
  b: 2,
  c: {
    d: 1,
    e: 8
  }
})

let simple = ref('你好，世界')

let myHot = reactive([1, 2, 3, 4])

let addBtn = function () {
  obj.a += 1
}

let addWraper = () => {
  obj.c.d += 1
}

let editSim = () => {
  // 使用ref包裹的数据，增加了一个value
  // ref 用于简单数据类型：
  // reactive用于对象
  simple.value = '我喜欢你好久了'
}

let editArray = () => {
  // 所以数组也用reactive
  myHot.push(111111)
  myHot[0] = 888
}
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
