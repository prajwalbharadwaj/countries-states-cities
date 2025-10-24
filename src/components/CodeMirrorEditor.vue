<script setup>
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import { EditorState } from '@codemirror/state'
import { EditorView, basicSetup } from 'codemirror'
import { json } from '@codemirror/lang-json'

const props = defineProps({
  jsonCode: { type: String, default: '' }
})

const editorContainer = ref(null)
let view = null

onMounted(() => {
  view = new EditorView({
    state: EditorState.create({
      doc: props.jsonCode,
      extensions: [
        basicSetup,
        json(),
        EditorState.readOnly.of(true)
      ]
    }),
    parent: editorContainer.value
  })
})

watch(() => props.jsonCode, (newValue) => {
  const currentValue = view.state.doc.toString()
  if (newValue !== currentValue) {
    view.dispatch({
      changes: { from: 0, to: currentValue.length, insert: newValue }
    })
  }
})

onBeforeUnmount(() => {
  if (view) view.destroy()
})
</script>

<template>
  <div ref="editorContainer" class="code-editor"></div>
</template>

<style scoped>
.code-editor {
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 14px;
  height: 300px;
  overflow: scroll;
}
</style>
