<template>
  <div class="relative" :class="marginBottom">
    <label v-if="label" :for="id?id:name"
           :class="[theme.RichTextAreaInput.label, {'uppercase text-xs':uppercaseLabels, 'text-sm':!uppercaseLabels}]"
    >
      {{ label }}
      <span v-if="required" class="text-red-500 required-dot">*</span>
    </label>
    <vue-editor :id="id?id:name" ref="editor" v-model="compVal" :disabled="disabled"
                :placeholder="placeholder" :class="[{ 'ring-red-500 ring-2': form && form.errors.has(name) }, theme.RichTextAreaInput.input]"
                :editor-toolbar="editorToolbar" class="rich-editor resize-y"
                :style="inputStyle"
    />

    <small v-if="help" :class="theme.RichTextAreaInput.help" v-text="help" />
    <has-error v-if="form" :form="form" :field="name" />
  </div>
</template>

<script>
import { VueEditor } from 'vue2-editor'
import inputMixin from '~/mixins/forms/input'

export default {
  name: 'RichTextAreaInput',
  components: { VueEditor },
  mixins: [inputMixin],

  props: {
    editorToolbar: {
      type: Array,
      default: () => {
        return [
          [{ header: 1 }, { header: 2 }],
          ['bold', 'italic', 'underline', 'link'],
          [{ list: 'ordered' }, { list: 'bullet' }]
        ]
      }
    }
  },

  data: () => ({}),

  computed: {},

  watch: {},

  created () {}
}
</script>

<style lang="scss">
.rich-editor {
  .ql-container {
    border-bottom: 0px !important;
    border-right: 0px !important;
    border-left: 0px !important;

    .ql-editor {
      min-height: 100px !important;
    }
  }

  .ql-toolbar {
    border-top: 0px !important;
    border-right: 0px !important;
    border-left: 0px !important;
  }

  .ql-snow .ql-toolbar .ql-picker-item.ql-selected, .ql-snow .ql-toolbar .ql-picker-item:hover, .ql-snow .ql-toolbar .ql-picker-label.ql-active, .ql-snow .ql-toolbar .ql-picker-label:hover, .ql-snow .ql-toolbar button.ql-active, .ql-snow .ql-toolbar button:focus, .ql-snow .ql-toolbar button:hover, .ql-snow.ql-toolbar .ql-picker-item.ql-selected, .ql-snow.ql-toolbar .ql-picker-item:hover, .ql-snow.ql-toolbar .ql-picker-label.ql-active, .ql-snow.ql-toolbar .ql-picker-label:hover, .ql-snow.ql-toolbar button.ql-active, .ql-snow.ql-toolbar button:focus, .ql-snow.ql-toolbar button:hover {
    @apply text-nt-blue;
  }
}
</style>
