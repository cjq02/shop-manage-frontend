<template>
  <div class="img-box">
    <el-image
      v-for="(item, itemIndex) in list"
      :key="item.id"
      class="img"
      :src="apiUtils.getFullFileUrl(item.fileUrl)"
      :zoom-rate="1.2"
      :max-scale="7"
      :min-scale="0.2"
      :preview-src-list="
        __.map(list, (file) => {
          return apiUtils.getFullFileUrl(file.fileUrl)
        })
      "
      :initial-index="itemIndex"
      fit="cover"
    ></el-image>
  </div>
</template>
<script lang="ts" setup>
defineProps<{
  list?: any[]
}>()
</script>
<style lang="scss" scoped>
.img {
  padding: 5px;
  width: 100px;
  height: 100px;
}
</style>
