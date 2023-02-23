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
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, ref, toRefs } from 'vue'
interface Member {
  id: number
  name: string
  age: number
  gender: string
}
export default defineComponent({
  setup() {
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

    const userInfoRefs = toRefs(userInfo)
    const uids: number[] = reactive([1, 2, 3])

    setTimeout(() => {
      userInfo.id = 2
      userInfo.name = 'Tom'
      userInfo.age = 20
    }, 2000)
    // 模板中使用需要return 页面加载时使用不需要return
    function update() {
      userInfo.id = 3
      userInfo.name = 'Tom3'
      userInfo.age = 21
    }

    const delayUpdate = () => {
      console.log('init')
    }

    onMounted(() => {
      delayUpdate()
    })
    return {
      msg,
      count,
      isVip,
      userInfo,
      update,
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
