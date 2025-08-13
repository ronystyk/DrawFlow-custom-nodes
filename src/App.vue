<template>
    <div style="width: 100vw; height: 100vh">
        <LoadingOverlay 
            :loading="loading" 
            :initTime="initTime" 
        />
        
        <MetricsPanel 
            :initTime="initTime"
            :paneReadyTime="paneReadyTime"
            :totalAppTime="totalAppTime"
            :memoryUsage="memoryUsage"
            :memoryInfo="memoryInfo"
            :cpuInfo="cpuInfo"
            :performanceMetrics="performanceMetrics"
            :cpuUsage="cpuUsage"
            :frameTime="frameTime"
        />

        <div ref="drawflowRef" id="drawflow"></div>
    </div>
</template>

<script setup>
import { ref, shallowRef, onMounted } from 'vue'
import * as Vue from 'vue'
import LoadingOverlay from './components/LoadingOverlay.vue'
import MetricsPanel from './components/MetricsPanel.vue'
import { usePerformanceMetrics } from './composables/usePerformanceMetrics'

import Drawflow from 'drawflow'
import 'drawflow/dist/drawflow.min.css'
import './styles/customs-drawflows.css'
import FormNode from './nodes/FormNode.vue'
import StartNode from './nodes/StartNode.vue'
import EndNode from './nodes/EndNode.vue'

const drawflowRef = ref();
const editor = shallowRef();

onMounted(() => {
  const el = drawflowRef.value
  if (!el) return
  // @ts-ignore
  editor.value = new Drawflow(el, Vue)
  
  editor.value.start()  

  buildElements()

  onPaneReady()
})

// Usar el composable de métricas de rendimiento
const {
    initTime,
    loading,
    paneReadyTime,
    totalAppTime,
    memoryUsage,
    memoryInfo,
    cpuInfo,
    performanceMetrics,
    cpuUsage,
    frameTime,
    onPaneReady
} = usePerformanceMetrics()


const buildElements = (cols = 10, rowsByElement = 10) => {    
    // Start Node
    editor.value.registerNode('StartNode', StartNode, {})
    editor.value.addNode('Start', 0, 1, 100, 400, 'auto-size-node', {}, 'StartNode', 'vue')

    // End Node
    editor.value.registerNode('EndNode', EndNode, {})
    editor.value.addNode('End', 1, 0, 200 + ((cols + 1) * 400), 100 +(rowsByElement + 1) * 400, 'auto-size-node', {}, 'EndNode', 'vue')

    for (let i = 1; i <= cols; i++) {
        for (let j = 1; j <= rowsByElement; j++) {
            editor.value.registerNode('FormNode', FormNode, {id: `input-${i}-${j}`, data: {label: `Form ${i}-${j}`}})
            editor.value.addNode(`input-${i}-${j}`, 1, 1,  100 + i * 400 , 100 + j * 400, 'auto-size-node', {}, 'FormNode', 'vue')
            const nextNode = ((rowsByElement * (i - 1)) + (j + 1)) + 1
            editor.value.addConnection(1,nextNode,'output_1','input_1')
            editor.value.addConnection(nextNode,2,'output_1','input_1')
        }
    }

    
}

</script>
