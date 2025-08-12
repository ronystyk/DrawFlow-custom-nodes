<template>
    <div class="start-node"> 
        <span class="node-text">Inicio</span>
    </div>
</template>

<script setup>
// No necesitamos props para este nodo simple
</script>

<style scoped>
.start-node {
    padding: 12px 20px;
    border-radius: 8px;
    border: 2px solid #4CAF50;
    color: #4CAF50;
    font-weight: 600;
    font-size: 14px;
    text-align: center;
    min-width: 80px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
}

.node-text {
    margin: 0;
}
</style>
