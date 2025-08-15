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

        <!-- Pane de Drawflow -->
        <div ref="drawflowRef" id="drawflow"></div>
    </div>
</template>

<script setup>
// Importaciones de Vue
import { ref, shallowRef, onMounted } from 'vue'
import * as Vue from 'vue'
import LoadingOverlay from './components/LoadingOverlay.vue'
import MetricsPanel from './components/MetricsPanel.vue'
import { usePerformanceMetrics } from './composables/usePerformanceMetrics'

// Importaciones de Drawflow
import Drawflow from 'drawflow'
import 'drawflow/dist/drawflow.min.css'
import './styles/customs-drawflows.css'

// Importaciones de nodos personalizados
import FormNode from './nodes/FormNode.vue'
import StartNode from './nodes/StartNode.vue'
import EndNode from './nodes/EndNode.vue'

// Referencias a los elementos de Vue
const drawflowRef = ref();
const editor = shallowRef();

// Función para inicializar el drawflow
onMounted(async () => {
  const el = drawflowRef.value
  if (!el) return
  editor.value = new Drawflow(el, Vue)
  
  editor.value.start()  

  editor.value.zoom_min = 0.3
  editor.value.zoom_max = 2

  buildElements().then(() => {
    onPaneReady()
  })
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

// Función para construir los elementos del drawflow
const buildElements = async (cols = 20, rowsByElement = 20) => {    
    // Registrar nodos
    editor.value.registerNode('StartNode', StartNode, {})
    editor.value.registerNode('EndNode', EndNode, {})
    editor.value.registerNode('FormNode', FormNode, {})

    // Nodo personalizado de inicio
    editor.value.addNode('Start', 0, 1, 100, 400, 'auto-size-node', {}, 'StartNode', 'vue')

    // Nodo personalizado de fin
    editor.value.addNode('End', 1, 0, 200 + ((cols + 1) * 400), 100 +(rowsByElement + 1) * 400, 'auto-size-node', {}, 'EndNode', 'vue')

    for (let i = 1; i <= cols; i++) {
        for (let j = 1; j <= rowsByElement; j++) {
            const nodeId = ((rowsByElement * (i - 1)) + (j + 1)) + 1
            // Nodo personalizado de formulario con datos iniciales
            const formData = {
                label: `Formulario ${i}-${j}`,
                name: 'Formulario',
                age: (i + j) * 10,
                email: 'email@ejemplo.com',
                option: 'opcion1',
            }
            editor.value.addNode(`input-${i}-${j}`, 1, 1,  100 + i * 400 , 100 + j * 400, 'auto-size-node', formData, 'FormNode', 'vue')

            // Conectar nodos
            editor.value.addConnection(1,nodeId,'output_1','input_1')
            editor.value.addConnection(nodeId,2,'output_1','input_1')
        }
    }
}

</script>