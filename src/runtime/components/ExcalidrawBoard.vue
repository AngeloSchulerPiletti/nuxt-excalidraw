<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import ExcalidrawDefault from '@excalidraw/excalidraw'
import React from 'react'
import { type Root, createRoot } from 'react-dom/client'
import type { Ref } from 'vue'
import type { ExcalidrawProps } from '@excalidraw/excalidraw/types/types'

export interface Props extends ExcalidrawProps {}

const props = defineProps<Props>()

const excalidrawRef: Ref<HTMLDivElement | null> = ref(null)
let root: null | Root = null

onMounted(async () => {
  if (excalidrawRef.value) {
    root = createRoot(excalidrawRef.value)
    let _Excalidraw: any
    if (ExcalidrawDefault) {
      _Excalidraw = ExcalidrawDefault
    }
    else {
      const Excalidraw = await import('@excalidraw/excalidraw')
      _Excalidraw = Excalidraw.Excalidraw
    }

    root.render(React.createElement(_Excalidraw, props as any))
  }
})

onBeforeUnmount(() => {
  if (root)
    root.unmount()
})
</script>

<template>
  <div ref="excalidrawRef" class="excalidraw-board" />
</template>

<style scoped>
.excalidraw-board {
  min-height: 100%;
  height: 90dvh;
}
</style>
