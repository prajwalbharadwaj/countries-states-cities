<script setup>
import { ref, onMounted, onBeforeUnmount, watch, nextTick } from 'vue'
import { EditorState } from '@codemirror/state'
import { EditorView, basicSetup } from 'codemirror'
import { json } from '@codemirror/lang-json'

// Props with better validation
const props = defineProps({
  jsonCode: {
    type: String,
    default: '',
    validator: (value) => typeof value === 'string'
  }
})

// Reactive references
const editorContainer = ref(null)
const editorView = ref(null)

// Editor configuration
const createEditorState = (content) => {
  return EditorState.create({
    doc: content,
    extensions: [
      basicSetup,
      json(),
      EditorState.readOnly.of(true)
    ]
  })
}

// Initialize editor
const initializeEditor = async () => {
  if (!editorContainer.value) return

  try {
    editorView.value = new EditorView({
      state: createEditorState(props.jsonCode),
      parent: editorContainer.value
    })
  } catch (error) {
    console.error('Failed to initialize CodeMirror editor:', error)
  }
}

// Update editor content
const updateEditorContent = async (newContent) => {
  if (!editorView.value) return

  const currentContent = editorView.value.state.doc.toString()
  if (newContent !== currentContent) {
    try {
      editorView.value.dispatch({
        changes: {
          from: 0,
          to: currentContent.length,
          insert: newContent
        }
      })
    } catch (error) {
      console.error('Failed to update editor content:', error)
    }
  }
}

// Lifecycle hooks
onMounted(async () => {
  await nextTick()
  await initializeEditor()
})

onBeforeUnmount(() => {
  if (editorView.value) {
    editorView.value.destroy()
    editorView.value = null
  }
})

// Watchers
watch(() => props.jsonCode, async (newValue) => {
  await nextTick()
  await updateEditorContent(newValue)
}, { immediate: false })
</script>

<template>
  <div ref="editorContainer" class="code-editor"></div>
</template>

<style scoped>
.code-editor {
  border: 1px solid #e1e5e9;
  border-radius: 8px;
  font-size: 14px;
  height: 300px;
  overflow: auto;
  background-color: #fafbfc;
  transition: border-color 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.code-editor:hover {
  border-color: #c1c8cd;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

.code-editor:focus-within {
  border-color: #007acc;
  box-shadow: 0 0 0 3px rgba(0, 122, 204, 0.1);
  outline: none;
}

/* Dark mode support */
@media (prefers-color-scheme: dark) {

  .code-editor:hover {
    border-color: #4a4a4a;
  }

  .code-editor:focus-within {
    border-color: #007acc;
    box-shadow: 0 0 0 3px rgba(0, 122, 204, 0.2);
  }
}

/* Responsive design */
@media (max-width: 768px) {
  .code-editor {
    height: 250px;
    font-size: 13px;
  }
}

/* Scrollbar styling */
.code-editor::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

.code-editor::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 4px;
}

.code-editor::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 4px;
}

.code-editor::-webkit-scrollbar-thumb:hover {
  background: #a8a8a8;
}
</style>
