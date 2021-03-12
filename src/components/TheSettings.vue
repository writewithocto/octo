<template>
  <div class="container mx-auto sm:px-4 mt-3 md:mt-12">
    <div>
      <h3 class="mb-3">Settings</h3>
      <h6 class="-mt-2 mb-0 text-gray-700 font-normal mb-3">Make yourself comfortable</h6>
      <Extendable scope="app.settings">
        <Editor/>
        <Theme/>
        <Encryption/>
        <Export/>
      </Extendable>
    </div>
  </div>
</template>

<script>
import Editor from '@/components/settings/Editor'
import Encryption from '@/components/settings/Encryption'
import Export from '@/components/settings/Export'
import Theme from '@/components/settings/Theme'

export default {
  name: 'TheSettings',
  components: {
    Editor,
    Encryption,
    Export,
    Theme,
  },
}
</script>
