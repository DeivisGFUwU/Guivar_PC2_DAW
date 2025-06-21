<template>
  <q-page class="flex flex-center">
    <q-card class="q-pa-xl shadow-2" style="width: 400px">
      <div class="text-h6 text-center text-primary q-mb-md">
        <q-icon name="trending_up" color="green" class="q-mr-sm" />
        Conversión de Monedas
      </div>
      <CurrencyConverter />
    </q-card>
  </q-page>
</template>

<script setup>
import CurrencyConverter from 'components/CurrencyConverter.vue'
</script>
