<script setup lang="ts">
import { nextTick, onBeforeUnmount, onMounted, ref } from 'vue'
import React from 'react'
import type { Root } from 'react-dom/client'
import type { Ref } from 'vue'
import type { ExcalidrawProps } from '@excalidraw/excalidraw/types/types'

export interface Props extends ExcalidrawProps {}

const props = defineProps<Props>()

const excalidrawRef: Ref<HTMLDivElement | null> = ref(null)
let root: null | Root = null

const ReactDOMClient = await import('react-dom/client')

onMounted(() => {
  nextTick(async () => {
    if (excalidrawRef.value) {
      root = ReactDOMClient.createRoot(excalidrawRef.value)
      const Excalidraw = (await import('@excalidraw/excalidraw')).Excalidraw

      root.render(React.createElement(Excalidraw, props as any))
    }
  })
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
