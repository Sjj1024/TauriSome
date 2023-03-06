<script setup lang="ts">
import { ref } from 'vue'
import { invoke } from '@tauri-apps/api/tauri'
import {
    StarOutlined,
    StarFilled,
    StarTwoTone,
    SketchCircleFilled,
} from '@ant-design/icons-vue'

const greetMsg = ref('')
const name = ref('')
const current = ref(6)

// 定义一个数据类型或者接口Option的对象，包含value并且它的值是string类型
interface Option {
    value: string
}

let value = ref('')
// options是一个Option类型的数组，所以()中填入的内容是符合规范的
let options = ref<Option[]>([
    { value: 'Burns Bay Road' },
    { value: 'Downing Street' },
    { value: 'Wall Street' },
])

// filterOption是一个函数，传入的参数是input和option，input是字符换，option是Option对象类型或接口
const filterOption = (input: string, option: Option) => {
    // return option.value.toUpperCase().indexOf(input.toUpperCase()) >= 0
    return option.value.toUpperCase().indexOf(input.toUpperCase()) >= 0
}

async function greet() {
    // Learn more about Tauri commands at https://tauri.app/v1/guides/features/command
    greetMsg.value = await invoke('greet', { name: name.value })
    console.log('按钮被点击了')
}
</script>

<template>
    <div class="card">
        <input id="greet-input" v-model="name" placeholder="Enter a name..." />
        <!-- <button type="button" @click="greet()">Greet</button> -->
        <a-button type="primary" @click="greet()">Primary Greet</a-button>
        <div>
            <star-outlined />
            <star-filled />
            <star-two-tone two-tone-color="#eb2f96" />
            <sketch-circle-filled />
        </div>
        <div>
            <a-dropdown>
                <a class="ant-dropdown-link" @click.prevent> Hover me </a>
                <template #overlay>
                    <a-menu>
                        <a-menu-item>
                            <a href="javascript:;">1st menu item</a>
                        </a-menu-item>
                        <a-menu-item>
                            <a href="javascript:;">2nd menu item</a>
                        </a-menu-item>
                        <a-menu-item>
                            <a href="javascript:;">3rd menu item</a>
                        </a-menu-item>
                    </a-menu>
                </template>
            </a-dropdown>
        </div>
        <div>
            <a-pagination v-model:current="current" :total="500" />
        </div>
        <div>
            <a-auto-complete
                v-model:value="value"
                :options="options"
                style="width: 200px"
                placeholder="input here"
                :filter-option="filterOption"
            />
        </div>
    </div>

    <p>{{ greetMsg }}</p>
</template>
