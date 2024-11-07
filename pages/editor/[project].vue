<template>
  <div class="w-full h-full">
    <div class="grid-container" @mousedown="startDrag" @mousemove="onDrag" @mouseup="stopDrag" @mouseleave="stopDrag"
      @wheel.prevent="onWheel">
      <div class="grid-background" :style="gridStyle"></div>
    </div>
    <div class="flex relative h-full">
      <WidgetBox />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const isDragging = ref(false)
const offset = ref({ x: 0, y: 0 })
const dragStart = ref({ x: 0, y: 0 })
const scale = ref(1)

const gridStyle = computed(() => ({
  transform: `translate(${offset.value.x}px, ${offset.value.y}px) scale(${scale.value})`
}))

const startDrag = (e) => {
  isDragging.value = true
  dragStart.value = {
    x: e.clientX - offset.value.x,
    y: e.clientY - offset.value.y
  }
}

const onDrag = (e) => {
  if (!isDragging.value) return
  offset.value = {
    x: e.clientX - dragStart.value.x,
    y: e.clientY - dragStart.value.y
  }
}

const stopDrag = () => {
  isDragging.value = false
}

const onWheel = (e) => {
  const delta = e.deltaY > 0 ? 0.9 : 1.1
  const newScale = scale.value * delta

  if (newScale >= 0.1 && newScale <= 5) {
    const rect = e.currentTarget.getBoundingClientRect()
    const mouseX = e.clientX - rect.left
    const mouseY = e.clientY - rect.top

    offset.value = {
      x: mouseX - (mouseX - offset.value.x) * delta,
      y: mouseY - (mouseY - offset.value.y) * delta
    }

    scale.value = newScale
  }
}
</script>

<style scoped>
.grid-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  user-select: none;
}

.grid-background {
  position: absolute;
  top: -2000%;
  left: -2000%;
  right: -2000%;
  bottom: -2000%;
  background-image:
    linear-gradient(var(--grid-color-major) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-color-major) 1px, transparent 1px),
    linear-gradient(var(--grid-color-minor) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-color-minor) 1px, transparent 1px);
  background-size:
    100px 100px,
    100px 100px,
    20px 20px,
    20px 20px;
  --grid-color-major: rgba(0, 0, 0, 0.1);
  --grid-color-minor: rgba(0, 0, 0, 0.05);
  width: 5000%;
  height: 5000%;
  transform-origin: 0 0;
}
</style>
