<template>
  <div class="q-pa-md flex flex-center column text-center">
    <q-icon name="error_outline" size="64px" color="red-5" />
    <h1 class="text-h4 q-mt-md">404 - Página no encontrada</h1>
    <p class="q-mt-sm">La página que buscas no existe o ha sido movida.</p>
    <q-btn class="q-mt-md" color="primary" label="Ir al inicio" to="/" />
  </div>
</template>

<script setup>
// No se requiere lógica adicional para esta página de error
</script>

<style scoped>
.flex {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 60vh;
}
</style>
