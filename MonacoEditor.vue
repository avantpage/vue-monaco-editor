<template>
  <div ref="editor" :class="customClass" :style="customStyle" />
</template>

<script>
import * as monaco from "monaco-editor/esm/vs/editor/editor.api";

export default {
  name: "MonacoEditor",
  props: {
    customStyle: { type: String, default: "" },
    customClass: { type: String, default: "m-editor" },
    value: { type: String, default: '{}' },
    options: {
      type: Object,
      default: () => {
        return {
          language: 'javascript',
          fontSize: '16px',
          automaticLayout: true,
          theme: 'vs-dark',
          roundedSelection: false,
          scrollBeyondLastLine: false,
          autoIndent: true,
          formatOnPaste: true,
          formatOnType: true,
          value: '{}',
        };
      },
    },
  },
  data() {
    return {
      editor: {},
    };
  },
  methods: {
    _onChange() {
      this.editor.getModel().onDidChangeContent(() => {
        this.$emit("value", { this.editor.getValue() });
      });
    },
  },
  mounted() {
    this.editor = monaco.editor.create(this.$refs["editor"], this.options);
    this.editor.setValue(this.value);
    this.editor.getAction('editor.action.formatDocument').run();
    this._onChange();
  },
  beforeDestroy() {
    this.editor && this.editor.dispose();
  },
};
</script>

<style scoped>
</style>

