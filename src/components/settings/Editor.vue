<template>
  <section>
    <h4 class="font-normal mt-3 md:mt-12">Editor</h4>
    <hr>
    <Extendable scope="app.settings.editor">
      <div class="mb-4">
        <label for="config-tab-size">Tab length</label>
        <input v-model="tabSize" type="number" min="2" id="config-tab-size" class="block appearance-none w-full py-1 px-2 mb-1 text-base leading-normal bg-white text-gray-800 border border-gray-200 rounded">
        <small class="text-gray-700">Number of spaces per tab (minimum: 2)</small>
      </div>
      <div class="mb-4">
        <label for="config-key-map">Keymaps</label>
        <div>
          <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle">
            <div class="custom-control custom-radio flex items-center">
              <input v-model="keyMap" type="radio" value="default" class="custom-control-input flex">
              <span class="custom-control-label flex">Default</span>
            </div>
          </label>
          <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle ml-2">
            <div class="custom-control custom-radio flex items-center">
              <input v-model="keyMap" type="radio" value="vim" class="custom-control-input flex">
              <span class="custom-control-label flex">Vim</span>
            </div>
          </label>
        </div>
        <small class="text-gray-700">Select an alternate keymapping</small>
      </div>
      <div class="mb-4">
        <h5 class="font-normal">Images</h5>
        <div class="mb-4">
          <p>This setting determines whether or not image tags (e.g. <code class="text-gray-700">![alt text](/path/to/image)</code>) will render images in your documents.</p>
        </div>
        <div class="mb-4">
          <div>
            <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle">
              <div class="custom-control custom-checkbox flex items-center">
                <input v-model="imagesEnabled" type="checkbox" class="custom-control-input flex">
                <span class="custom-control-label flex">Enable Images</span>
              </div>
            </label>
            <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle ml-2">
              <div class="custom-control custom-checkbox flex items-center">
                <input v-model="showCaptions" :disabled="!imagesEnabled" type="checkbox" class="custom-control-input flex">
                <span class="custom-control-label flex">Show Captions</span>
              </div>
            </label>
          </div>
          <small class="text-gray-700">Note: Captions are pulled from alt text and rendered under your images in the image container.</small>
        </div>
      </div>
    </Extendable>
  </section>
</template>

<script>
import {
  SET_EDITOR_IMAGES_ENABLED,
  SET_EDITOR_IMAGES_SHOW_CAPTIONS,
  SET_EDITOR_TAB_SIZE,
  SET_EDITOR_KEY_MAP,
} from '@/store/modules/settings';

export default {
  name: 'Editor',
  computed: {
    imagesEnabled: {
      get() {
        return this.$store.state.settings.editor.images.enabled;
      },
      set(value) {
        this.$store.dispatch(SET_EDITOR_IMAGES_ENABLED, value);
      },
    },
    keyMap: {
      get() {
        return this.$store.state.settings.editor.keyMap;
      },
      set(value) {
        this.$store.dispatch(SET_EDITOR_KEY_MAP, value);
      },
    },
    showCaptions: {
      get() {
        return this.$store.state.settings.editor.images.showCaptions;
      },
      set(value) {
        this.$store.dispatch(SET_EDITOR_IMAGES_SHOW_CAPTIONS, value);
      },
    },
    tabSize: {
      get() {
        return this.$store.state.settings.editor.tabSize;
      },
      set(value) {
        this.$store.dispatch(SET_EDITOR_TAB_SIZE, parseInt(value) || 2);
      },
    },
  },
};
</script>
