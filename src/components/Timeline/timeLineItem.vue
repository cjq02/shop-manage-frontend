<template>
  <div class="timeline-item">
    <div v-if="!isLast" class="line"></div>
    <div class="dot">
      <el-button :type="type" circle size="small" />
    </div>
    <slot></slot>
  </div>
</template>
<script lang="ts" setup>
defineProps<{
  type: any
  isLast: boolean
}>()
</script>
<style lang="scss" scoped>
$dot_diameter: 16px;
.timeline-item {
  display: flex;
  position: relative;
  align-items: center;
  padding: 10px;
  .line {
    position: absolute;
    left: 17px;
    top: 27px;
    border-left: 2px solid silver;
    height: 100%;
  }
  .dot {
    align-self: flex-start;
    width: 40px;
    :deep(.el-button) {
      width: $dot_diameter;
      height: $dot_diameter;
    }
  }
}
</style>
