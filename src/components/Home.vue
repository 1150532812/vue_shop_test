<template>
  <div>
    <el-button type="info" @click="logout">退出</el-button>
  </div>

</template>

<script>
export default {
  name: 'Home',
  methods: {
    logout () {
      // 退出功能 清空token，重定向到login
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang="less" scoped>

</style>
