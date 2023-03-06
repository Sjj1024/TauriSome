<script setup lang="ts">
import { ref } from "vue";
import { invoke } from "@tauri-apps/api/tauri";

const greetMsg = ref("");
const name = ref("");

async function greet() {
  // Learn more about Tauri commands at https://tauri.app/v1/guides/features/command
  // 先判断是不是tauri环境还是浏览器环境
  greetMsg.value = await invoke("greet", { name: name.value });
  console.log("按钮备件第几了");
  
}
</script>

<template>
  <div class="card">
    <input id="greet-input" v-model="name" placeholder="Enter a name..." />
    <!-- <button type="button" @click="greet()">Greet</button> -->
    <a-button type="primary" @click="greet()">Primary Greet</a-button>
  </div>

  <p>{{ greetMsg }}</p>
</template>
