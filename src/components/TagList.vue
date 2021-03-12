<template>
  <div class="container mx-auto sm:px-4 flex flex-col py-3">
    <Tag class="tag inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-gray-600 text-white hover:bg-gray-700 p-6" v-for="tag in tags" :key="tag" :tag="tag"></Tag>
  </div>
</template>

<script>
import Tag from '@/components/Tag.vue';

export default {
  name: 'TagList',
  components: {
    Tag,
  },
  computed: {
    tags() {
      return this.$store.getters.tags;
    },
  },
};
</script>

<style scoped>
.tag {
  align-items: center;
  display: flex;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
}

.tag:first-child {
  margin-top: 1rem;
}

.hr {
  background-color: var(--gray-dark, grey);
  height: 0.125rem;
  margin: 0.75rem 0.75rem 0.75rem 1rem;
}
</style>
