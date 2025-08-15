<template>
    <div class="input-node"> 
        <div class="node-header">
            <span class="node-icon">📋</span>
            <input class="node-title" df-label disabled/>
        </div>

        <div class="node-content">
            <div class="input-group">
                <label>Nombre:</label>
                <input type="text" placeholder="Ingresa tu nombre" df-name/>
            </div>

            <div class="input-group">
                <label>Edad:</label>
                <input type="number" placeholder="0" df-age/>
            </div>

            <div class="input-group">
                <label>Email:</label>
                <input type="email" placeholder="email@ejemplo.com" df-email/>
            </div>

            <div class="input-group">
                <label>Seleccionar:</label>
                <select df-option>
                    <option value="">Selecciona una opción</option>
                    <option value="opcion1">Opción 1</option>
                    <option value="opcion2">Opción 2</option>
                    <option value="opcion3">Opción 3</option>
                </select>
            </div>
        </div>

    </div>
</template>

<script setup>
</script>

<style scoped>
.input-node {
    padding: 15px;
    border-radius: 8px;
    border: 2px solid #42b883;
    background: white;
    min-width: 200px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.node-header {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 12px;
    font-weight: bold;
    color: #42b883;
}

.node-icon {
    font-size: 18px;
}

.node-title {
    font-size: 14px;
    border: none;
    outline: none;
    background: transparent;
    width: 100%;
    text-align: left;
}

.node-content {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.input-group {
    display: flex;
    flex-direction: column;
    gap: 4px;
}

.input-group label {
    font-size: 12px;
    font-weight: 500;
    color: #666;
}

.input-group input,
.input-group select {
    padding: 6px 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 12px;
    background: #f9f9f9;
}

.input-group input:focus,
.input-group select:focus {
    outline: none;
    border-color: #42b883;
    background: white;
}

.input-group input[type="checkbox"] {
    width: 100%;
}
</style>