<template>
  <p class="msg">{{ msg }}</p>
  <p ref="meesage">测试ref用,ref也可以和Vue2 一致</p>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'

export default defineComponent({
  setup() {
    const msg = ref<string>('Hello World')
    // 数值
    const count = ref<number>(1)

    // 布尔值
    const isVip = ref<boolean>(false)

    interface Member {
      id: number
      name: string
    }

    const userInfo: Member = reactive({
      id: 1,
      name: 'Tom',
    })

    const uids: number[] = reactive([1, 2, 3])
    return {
      msg,
      count,
      isVip,
      userInfo,
    }
  },
})
</script>

<style>
.msg {
  font-size: 18px;
}
</style>
