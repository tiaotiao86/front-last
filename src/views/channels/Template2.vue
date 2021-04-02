<template>
  <div class="layui-container">
    <div class="layui-row layui-col-space15">
      <div class="layui-col-md8">
        <imooc-list></imooc-list>
      </div>

    </div>
  </div>
</template>

<script>
import List from '@/components/contents/List'
export default {
  name: 'template1',
  components: {
    'imooc-list': List
  }
}
</script>

<style lang="scss" scoped>
</style>
