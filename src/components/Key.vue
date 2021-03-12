<template>
  <small class="key border border-gray-500 rounded uppercase leading-none text-xs hidden py-1 px-1.5 md:block" :class="{ active: active }">
    <slot></slot>
  </small>
</template>

<script>
export default {
  name: 'Key',
  props: {
    active: {
      default: false,
      required: false,
      type: Boolean,
    },
  },
}
</script>
