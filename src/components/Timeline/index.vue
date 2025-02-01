<template>
  <ul>
    <li v-for="(item, index) in data" :key="index">
      <slot :item="item" :isLast="index === data.length - 1"></slot>
    </li>
  </ul>
</template>
<script lang="ts" setup>
withDefaults(
  defineProps<{
    data: any
  }>(),
  {
    data: [],
  },
)
</script>
