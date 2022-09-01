<template>
  <div>
    <el-tree
      ref="treeRef"
      :data="list"
      show-checkbox
      node-key="roleId"
      :check-strictly="true"
      :default-checked-keys="authority"
      :props="{ children: 'roleList', label: 'name' }"
    />
    <el-button @click="changeAuthority">确认修改</el-button>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, reactive, toRefs } from 'vue'
import { useRoute } from 'vue-router'
import { InitData } from '@/type/authority'
import { getAuthorityList } from '../request/api'

export default defineComponent({
  setup() {
    const route = useRoute()
    const params: any = route.params
    const data = reactive(new InitData(params.id, params.authority))
    onMounted(() => {
      getAuthorityList().then((res) => {
        data.list = res.data
      })
    })
    const changeAuthority = () => {
      const arr = data.treeRef
        .getCheckedKeys()
        .sort(function (a: number, b: number) {
          return a - b
        })
      console.log(arr)
    }
    return { ...toRefs(data), changeAuthority }
  }
})
</script>

<style scoped></style>
