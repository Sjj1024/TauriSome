<script setup lang="ts">
import { ref, reactive, watch } from 'vue'
import { invoke } from '@tauri-apps/api/tauri'
import {
    StarOutlined,
    StarFilled,
    StarTwoTone,
    SketchCircleFilled,
} from '@ant-design/icons-vue'
import dayjs, { Dayjs } from 'dayjs'
import type { UploadChangeParam } from 'ant-design-vue'
import { message } from 'ant-design-vue'
import { UploadOutlined } from '@ant-design/icons-vue'
import { LeftCircleOutlined, RightCircleOutlined } from '@ant-design/icons-vue'

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

const plainOptions = ['Apple', 'Pear', 'Orange']

const state = reactive({
    indeterminate: true,
    checkAll: false,
    checkedList: ['Apple', 'Orange'],
})

const onCheckAllChange = (e: any) => {
    Object.assign(state, {
        checkedList: e.target.checked ? plainOptions : [],
        indeterminate: false,
    })
}
watch(
    () => state.checkedList,
    (val) => {
        state.indeterminate = !!val.length && val.length < plainOptions.length
        state.checkAll = val.length === plainOptions.length
    }
)

const dateFormat = 'YYYY/MM/DD'

let value1 = ref<Dayjs>(dayjs('2015/01/01', dateFormat))

interface FormState {
    username: string
    password: string
    remember: boolean
}
const formState = reactive<FormState>({
    username: '',
    password: '',
    remember: true,
})

const onFinish = (values: any) => {
    console.log('Success:', values)
}

const onFinishFailed = (errorInfo: any) => {
    console.log('Failed:', errorInfo)
}

const pingFen = ref<number>(2)

const huaDong = ref<[number, number]>([20, 50])

const disabled = ref<boolean>(false)
const checked = ref<boolean>(false)

const handleChange = (info: UploadChangeParam) => {
    if (info.file.status !== 'uploading') {
        console.log(info.file, info.fileList)
    }
    if (info.file.status === 'done') {
        message.success(`${info.file.name} file uploaded successfully`)
    } else if (info.file.status === 'error') {
        message.error(`${info.file.name} file upload failed.`)
    }
}
const fileList = ref([])
const headers = {
    authorization: 'authorization-text',
}
</script>

<template>
    <div class="card">
        <input id="greet-input" v-model="name" placeholder="Enter a name..." />
        <!-- <button type="button" @click="greet()">Greet</button> -->
        <a-button type="primary" @click="greet()">Primary Greet</a-button>
        <a-row>
            <a-col :span="12">col-12</a-col>
            <a-col :span="12">
                <div>
                    <star-outlined />
                    <star-filled />
                    <star-two-tone two-tone-color="#eb2f96" />
                    <sketch-circle-filled /></div
            ></a-col>
        </a-row>
        <a-row>
            <a-col></a-col>
        </a-row>
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
        <div>
            <a-cascader
                v-model:value="value"
                :options="options"
                placeholder="Please select"
            />
        </div>
        <div>
            多选选项：
            <div>
                <a-checkbox
                    v-model:checked="state.checkAll"
                    :indeterminate="state.indeterminate"
                    @change="onCheckAllChange"
                >
                    Check all
                </a-checkbox>
            </div>
            <a-divider />
            <a-checkbox-group
                v-model:value="state.checkedList"
                :options="plainOptions"
            />
        </div>
        <div>
            日期选择器：<a-date-picker
                v-model:value="value1"
                :format="dateFormat"
            />
        </div>
        <div>
            <a-form
                :model="formState"
                name="basic"
                :label-col="{ span: 8 }"
                :wrapper-col="{ span: 16 }"
                autocomplete="off"
                @finish="onFinish"
                @finishFailed="onFinishFailed"
            >
                <a-form-item
                    label="Username"
                    name="username"
                    :rules="[
                        {
                            required: true,
                            message: 'Please input your username!',
                        },
                    ]"
                >
                    <a-input v-model:value="formState.username" />
                </a-form-item>

                <a-form-item
                    label="Password"
                    name="password"
                    :rules="[
                        {
                            required: true,
                            message: 'Please input your password!',
                        },
                    ]"
                >
                    <a-input-password v-model:value="formState.password" />
                </a-form-item>

                <a-form-item
                    name="remember"
                    :wrapper-col="{ offset: 8, span: 16 }"
                >
                    <a-checkbox v-model:checked="formState.remember"
                        >Remember me</a-checkbox
                    >
                </a-form-item>

                <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
                    <a-button type="primary" html-type="submit"
                        >Submit</a-button
                    >
                </a-form-item>
            </a-form>
        </div>
        <div>评分：<a-rate v-model:value="pingFen" /></div>
        <div>
            滑动条；<a-slider
                v-model:value="huaDong"
                range
                :disabled="disabled"
            />
        </div>
        <div>切换按钮：<a-switch v-model:checked="checked" /></div>
        <div>
            <a-upload
                v-model:file-list="fileList"
                name="file"
                action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
                :headers="headers"
                @change="handleChange"
            >
                <a-button>
                    <upload-outlined></upload-outlined>
                    Click to Upload
                </a-button>
            </a-upload>
        </div>
        <div>
            <a-carousel arrows autoplay>
                <template #prevArrow>
                    <div
                        class="custom-slick-arrow"
                        style="left: 10px; z-index: 1"
                    >
                        <left-circle-outlined />
                    </div>
                </template>
                <template #nextArrow>
                    <div class="custom-slick-arrow" style="right: 10px">
                        <right-circle-outlined />
                    </div>
                </template>
                <div><h3>1</h3></div>
                <div><h3>2</h3></div>
                <div><h3>3</h3></div>
                <div><h3>4</h3></div>
            </a-carousel>
        </div>
        <div>
            <a-progress type="circle" :percent="75" />
            <a-progress type="circle" :percent="70" status="exception" />
            <a-progress type="circle" :percent="100" />
        </div>
    </div>

    <p>{{ greetMsg }}</p>
</template>

<style scoped>
/* For demo */
.ant-carousel :deep(.slick-slide) {
    text-align: center;
    height: 160px;
    line-height: 160px;
    background: #364d79;
    overflow: hidden;
}

.ant-carousel :deep(.slick-arrow.custom-slick-arrow) {
    width: 25px;
    height: 25px;
    font-size: 25px;
    color: #fff;
    background-color: rgba(31, 45, 61, 0.11);
    opacity: 0.3;
    z-index: 1;
}
.ant-carousel :deep(.custom-slick-arrow:before) {
    display: none;
}
.ant-carousel :deep(.custom-slick-arrow:hover) {
    opacity: 0.5;
}

.ant-carousel :deep(.slick-slide h3) {
    color: #fff;
}
</style>
