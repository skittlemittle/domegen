<script setup>
import Controls from './components/Controls.vue'

import { onBeforeMount, onMounted, ref, watch } from 'vue'
const radius = ref(5)
const zoom = ref(40)
const ellipseHeight = ref(4)
const ellipseWidth = ref(7)
const showEllipse = ref(false)
const showCircle = ref(true)
const showGrid = ref(true)
const panelRef = ref(null)

const newRender = () => {
  drawDome(showCircle.value, showEllipse.value,
    {
      radius: radius.value,
      height: ellipseHeight.value,
      width: ellipseWidth.value
    })
}

const updateZoom = (newZoom) => {
  changeZoom(newZoom)
  newRender()
}

watch(radius, newRender)
watch(zoom, updateZoom)
watch(ellipseHeight, newRender)
watch(ellipseWidth, newRender)
watch(showEllipse, newRender)
watch(showCircle, newRender)
watch(showGrid, () => {
  setGrid(showGrid.value)
  newRender()
})

const getCirclePlan = () => {
  return generateCirclePlan(bresenhamOctet(radius.value).map(it => it[1]))
}

const onWindowResize = () => {
  const appWidth = document.getElementById('app').offsetWidth
  const cutWidth = (appWidth > 50 * 16) ?
    panelRef.value.$refs.controlPanel.offsetWidth : 0;

  scaleCanvas(cutWidth)
  changeZoom(zoom.value)
  newRender()
}

onMounted(() => { window.addEventListener('resize', onWindowResize) })
onBeforeMount(() => { window.removeEventListener('resize', onWindowResize) })

// gotta wait for the script to load before we can call anything
window.addEventListener('load', onWindowResize)
</script>

<template>
  <div id="panel-container">
    <Controls ref="panelRef" v-model:radius="radius" v-model:zoom="zoom" v-model:height="ellipseHeight"
      v-model:width="ellipseWidth" v-model:showEllipse="showEllipse" v-model:showCircle="showCircle"
      v-model:showGrid="showGrid" />
    <p>
      {{ getCirclePlan().toString().replaceAll(',', ' ') }}
    </p>
  </div>

  <main id="canvas-container">
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

main {
  flex: 1;
  background-color: yellowgreen;
  overflow: hidden;
}

#panel-container p {
  padding: 1rem;
}
</style>
