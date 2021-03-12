<template>
  <div class="flex flex-col flex-grow">
    <div class="container mx-auto sm:px-4 max-w-full mx-auto sm:px-4 container min-w-xl mx-auto sm:px-4 flex flex-grow">
      <div class="editor flex flex-col flex-grow min-w-0" @click="focusEditor">
        <div class="gutter" @click="focusEditorStart"></div>
        <MarkdownEditor ref="editable" class="editable" :theme="theme" :initialCursor="initialCursor" :initialVimMode="initialVimMode" :settings="settings" :value="document.text" @input="input" @ready="onReady" />
        <div class="gutter flex-grow" @click="focusEditorEnd"></div>
      </div>
    </div>
    <div v-if="!showRightSidebar && currentDoc" class="fixed top-2 right-2 z-index-10 hidden md:block">
      <button @click="toggleMeta" class="inline-block align-middle text-center select-none border font-normal whitespace-no-wrap rounded py-1 px-3 leading-normal no-underline bg-gray-600 text-white hover:bg-gray-700">
        <InfoLabel>Info</InfoLabel>
      </button>
    </div>
  </div>
</template>

<script>
import Doc from '@/models/doc';

import InfoLabel from '@/components/labels/Info';
// figure out what is happening here...
import MarkdownEditor from '@voraciousdev/vue-markdown-editor';
import Tag from '@/components/Tag';

import {
  ADD_DOCUMENT,
  EDIT_DOCUMENT,
  SET_DOCUMENT,
  SET_EDITOR,
  SET_RIGHT_SIDEBAR_VISIBILITY,
} from '@/store/actions';

export default {
  name: 'TheEditor',
  components: {
    InfoLabel,
    MarkdownEditor,
    Tag,
  },
  props: {
    initialCursor: {
      type: Object,
      default: () => ({
        character: 0,
        line: 0,
      }),
      validator: (cursor) => (
        cursor.hasOwnProperty('character') && cursor.hasOwnProperty('line')
      ),
    },
    initialVimMode: {
      type: String
    },
  },
  data() {
    return {
      editor: null,
      placeholder: new Doc(),
    };
  },
  computed: {
    currentDoc() {
      return this.$store.getters.currentDoc;
    },
    document() {
      return this.$store.getters.decrypted.find(doc => doc.id === this.$route.params.id) || this.placeholder;
    },
    mediumPlus() {
      return ['md', 'lg', 'xl'].includes(this.$mq);
    },
    settings() {
      return this.$store.state.settings.editor;
    },
    showRightSidebar() {
      return this.$store.state.showRightSidebar;
    },
    theme() {
      return this.$store.state.settings.theme;
    },
  },
  methods: {
    async focusEditor() {
      this.$refs.editable.focus();
    },
    async focusEditorEnd() {
      this.$refs.editable.focusEnd();
    },
    async focusEditorStart() {
      this.$refs.editable.focusStart();
    },
    async input(text) {
      if (this.$route.params.id) {
        this.$store.dispatch(EDIT_DOCUMENT, { id: this.document.id, text });
      } else {
        this.$store.dispatch(ADD_DOCUMENT, new Doc({ id: this.document.id, text }));

        this.$router.push({
          name: 'document',
          params: {
            id: this.document.id,
            initialCursor: {
              character: this.editor.getCursor().ch,
              line: this.editor.getCursor().line,
            },
            initialVimMode: this.editor.getOption('keyMap'),
          },
        });
      }
    },
    async onReady(instance) {
      this.editor = instance;

      this.focusEditor();
      this.$store.dispatch(SET_EDITOR, this.editor);
    },
    async toggleMeta() {
      this.$store.dispatch(SET_RIGHT_SIDEBAR_VISIBILITY, !this.showRightSidebar);
    },
  },
  beforeRouteUpdate(to, from, next) {
    if (to.name === 'document') {
      this.$store.dispatch(SET_DOCUMENT, { id: to.params.id });
    }

    next();
  },
};
</script>

<style scoped>
  .md-plus .editable {
    font-size: 1.1em;
  }

  .editor .editable {
    outline: none;
    white-space: pre-wrap;
  }

  .editor .gutter {
    cursor: text;
    min-height: 1rem;
    width: 100%;
  }
</style>
