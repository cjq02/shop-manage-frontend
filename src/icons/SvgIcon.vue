<template>
  <svg :class="svgClass" aria-hidden="true">
    <use :xlink:href="iconName" />
  </svg>
</template>

<script>
import { computed, defineComponent } from 'vue'

export default defineComponent({
  props: {
    iconClass: {
      type: String,
      required: true,
    },
    className: {
      type: String,
      default: '',
    },
    // color: {
    //   type: String,
    //   default: '#889aa4',
    // },
  },
  setup(props) {
    return {
      iconName: computed(() => `#icon-${props.iconClass}`),
      svgClass: computed(() => {
        if (props.className) {
          return `svg-icon ${props.className}`
        }
        return 'svg-icon'
      }),
    }
  },
})
</script>

<style scope lang="scss">
.sub-el-icon,
.nav-icon {
  display: inline-block;
  position: relative;
  margin-right: 12px;
  font-size: 15px;
}
.svg-icon {
  position: relative;
  width: 1em;
  height: 1em;
  vertical-align: -2px;
  fill: currentColor;
}
</style>
