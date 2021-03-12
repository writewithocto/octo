<template>
  <section>
    <h4 class="font-normal mt-3 md:mt-12">Theme</h4>
    <hr>
    <Extendable scope="app.settings.theme">
      <div class="mb-4">
        <div>
          <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle">
            <div class="custom-control custom-radio flex items-center">
              <input v-model="theme" type="radio" value="dark" class="custom-control-input flex">
              <span class="custom-control-label flex">Dark</span>
            </div>
          </label>
          <label class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-blue-600 text-white hover:bg-blue-600 btn-toggle ml-2">
            <div class="custom-control custom-radio flex items-center">
              <input v-model="theme" type="radio" value="light" class="custom-control-input flex">
              <span class="custom-control-label flex">Light</span>
            </div>
          </label>
        </div>
      </div>
    </Extendable>
  </section>
</template>

<script>
import {
  SET_THEME,
} from '@/store/modules/settings';

export default {
  name: 'Theme',
  computed: {
    theme: {
      get() {
        return this.$store.state.settings.theme;
      },
      set(value) {
        this.$store.dispatch(SET_THEME, value);
      },
    },
  },
};
</script>
