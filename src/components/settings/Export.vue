<template>
  <section>
    <h4 class="font-normal mt-3 md:mt-12">Export Documents</h4>
    <hr>
    <div class="mb-4">
      <label>Bundle all documents as JSON. Documents will be decrypted if the necessary keys are available.</label>
    </div>
    <div class="mb-4">
      <router-link :to="{ name: 'export' }" class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-gray-600 text-white hover:bg-gray-700">Export Documents</router-link>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Export',
}
</script>
