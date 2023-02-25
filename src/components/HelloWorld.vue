<template>
  <p class="msg">{{ msg }}</p>
  <p ref="meesage">测试ref用,ref也可以和Vue2 一致</p>
  <h4>测试toRefs</h4>
  <ul class="user-info">
    <li class="item">
      <span class="key">ID:</span>
      <span class="value">{{ id }}</span>
    </li>

    <li class="item">
      <span class="key">name:</span>
      <span class="value">{{ name }}</span>
    </li>

    <li class="item">
      <span class="key">age:</span>
      <span class="value">{{ age }}</span>
    </li>

    <li class="item">
      <span class="key">gender:</span>
      <span class="value">{{ gender }}</span>
    </li>
  </ul>

  <!-- 在这里点击执行 `return` 出来的方法 -->
  <button @click="update">修改MSG</button>
  <Editor :value="mdValue" @change="handleChange" />
</template>

<script lang="ts">
import gfm from '@bytemd/plugin-gfm'
import { Editor, Viewer } from '@bytemd/vue-next'

import {
  defineComponent,
  onMounted,
  reactive,
  ref,
  toRefs,
  watch,
  computed,
} from 'vue'

const plugins = [
  gfm(),
  // Add more plugins here
]

interface Member {
  id: number
  name: string
  age: number
  gender: string
}
export default defineComponent({
  components: {
    // Editor,
  },
  setup() {
    const mdValue = ref<string>('')
    const msg = ref<string>('Hello World')
    // 数值
    const count = ref<number>(1)

    // 布尔值
    const isVip = ref<boolean>(false)

    const userInfo: Member = reactive({
      id: 1,
      name: 'Tom',
      age: 18,
      gender: 'male',
    })
    const index = ref<number>(0)
    const userInfoRefs = toRefs(userInfo)
    const uids: number[] = reactive([1, 2, 3])

    setTimeout(() => {
      userInfo.id = 2
      userInfo.name = 'Tom'
      userInfo.age = 20
      index.value++
      msg.value = 'hahahahahhahah'
    }, 2000)

    // 模板中使用需要return 页面加载时使用不需要return
    function update() {
      userInfo.id = 3
      userInfo.name = 'Tom3'
      userInfo.age = 21
    }
    function handleChange(v: string) {
      mdValue.value = v
    }
    const delayUpdate = () => {
      console.log('init')
    }
    // 练习watch
    // 基础用法
    watch(userInfo, () => {
      console.log('监听整个 userInfo', userInfo.name)
    })
    watch(
      () => userInfo.name,
      (newValue, oldValue) => {
        console.log('只监听 name 的变化', userInfo.name)
        console.log('打印前后变化的值', { oldValue, newValue })
      }
    )

    // 批量用法
    // 抽离公共处理行为
    const handleWatch = (
      newValue: string | number,
      oldValue: string | number
    ): void => {
      console.log(newValue, oldValue)
    }
    const foo = ref<string>('')

    setTimeout(() => {
      foo.value = 'Hello World!'
    }, 2000)

    function bar() {
      // debugger
      console.log(foo.value)
    }

    // 使用 watch 需要先手动执行一次
    // bar()

    // 然后当 foo 有变动时，才会通过 watch 来执行 bar()
    watch(foo, bar)
    watch(index, handleWatch)
    watch(msg, handleWatch)
    watch(
      // 数据源改成了数组
      [msg, index],
      // 回调的入参也变成了数组，每个数组里面的顺序和数据源数组排序一致
      ([newMessage, newIndex], [oldMessage, oldIndex]) => {
        console.log('message 的变化', { newMessage, oldMessage })
        console.log('index 的变化', { newIndex, oldIndex })
      }
    )

    onMounted(() => {
      delayUpdate()
    })
    return {
      msg,
      count,
      isVip,
      userInfo,
      mdValue,
      update,
      handleChange,
      ...userInfoRefs,
    }
  },
})
</script>

<style>
.msg {
  font-size: 18px;
}
</style>
