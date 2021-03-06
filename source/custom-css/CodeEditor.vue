<template>
  <textarea ref="custom-css"></textarea>
</template>

<script>
import CodeMirror from 'codemirror'
import 'codemirror/addon/edit/closebrackets'

import 'codemirror/lib/codemirror.css'
import 'codemirror/mode/css/css'

export default {
  name: 'CodeEditor',
  props: {
    contents: {
      type: String,
      default: 'hello world'
    }
  },
  data: function () {
    return {
      cmInstance: null
    }
  },
  watch: {
    contents: function () {
      if (this.cmInstance !== null) {
        this.cmInstance.setValue(this.contents)
      }
    }
  },
  mounted: function () {
    this.cmInstance = CodeMirror.fromTextArea(this.$refs['custom-css'], {
      lineNumbers: true,
      theme: 'code-editor',
      mode: 'css',
      cursorScrollMargin: 20,
      lineWrapping: true,
      autoCloseBrackets: true,
      // Disable cursor blinking, as we apply a @keyframes animation
      cursorBlinkRate: 0
    })

    this.cmInstance.setValue(this.contents)

    this.cmInstance.on('change', (event, changeObj) => {
      this.$emit('input', this.cmInstance.getValue())
    })
  },
  beforeDestroy: function () {
    const cmWrapper = this.cmInstance.getWrapperElement()
    // "Remove this from your tree to delete an editor instance."
    cmWrapper.parentNode.removeChild(cmWrapper)
  }
}
</script>

<style lang="less">
// We're using this solarized theme here: https://ethanschoonover.com/solarized/
@base03:    #002b36;
@base02:    #073642;
@base01:    #586e75;
@base00:    #657b83;
@base0:     #839496;
@base1:     #93a1a1;
@base2:     #eee8d5;
@base3:     #fdf6e3;
@yellow:    #b58900;
@orange:    #cb4b16;
@red:       #dc322f;
@magenta:   #d33682;
@violet:    #6c71c4;
@blue:      #268bd2;
@cyan:      #2aa198;
@green:     #859900;

body {
  .CodeMirror.cm-s-code-editor {
    margin: 20px 0px;
    background-color: white;
    border: 1px solid rgb(173, 173, 173);
    color: @base01;
    .cm-string     { color: @green; }
    .cm-string-2   { color: @green; }
    .cm-keyword    { color: @green; }
    .cm-atom       { color: @green; }
    .cm-tag        { color: @blue; }
    .cm-qualifier  { color: @blue; }
    .cm-builtin    { color: @blue; }
    .cm-variable-2 { color: @yellow; }
    .cm-variable   { color: @yellow; }
    .cm-comment    { color: @base1; }
    .cm-attribute  { color: @orange; }
    .cm-property   { color: @magenta; }
    .cm-type       { color: @red; }
    .cm-number     { color: @violet; }
    .CodeMirror-gutters { background-color: @base1; }
    .CodeMirror-linenumber { color: @base00; }
  }

  &.dark {
    .CodeMirror.cm-s-code-editor {
      background-color: rgb(65, 65, 65);
      border-color: rgb(100, 100, 100);
      color: @base01;
      .cm-string     { color: @red; }
      .cm-string-2   { color: @red; }
      .cm-keyword    { color: @red; }
      .cm-atom       { color: @red; }
      .cm-tag        { color: @blue; }
      .cm-qualifier  { color: @blue; }
      .cm-builtin    { color: @blue; }
      .cm-variable-2 { color: @yellow; }
      .cm-variable   { color: @yellow; }
      .cm-comment    { color: @base1; }
      .cm-attribute  { color: @orange; }
      .cm-property   { color: @magenta; }
      .cm-type       { color: @green; }
      .cm-number     { color: @violet; }
      .CodeMirror-gutters { background-color: @base01; }
      .CodeMirror-linenumber { color: @base1; }
    }
  }
}
</style>
