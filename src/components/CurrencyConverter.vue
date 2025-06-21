<template>
  <div class="q-gutter-md">
    <div class="text-subtitle2 q-mb-xs">Monto a convertir</div>
    <q-input v-model.number="amount" type="number" outlined />

    <div class="row q-col-gutter-md">
      <div class="col-6">
        <div class="text-weight-medium q-mb-xs">Desde</div>
        <q-select
          v-model="from"
          :options="currencyOptions"
          outlined
          emit-value
          map-options
          option-label="label"
          option-value="value"
          :display-value="formatDisplay(from)"
        >
          <template v-slot:option="scope">
            <q-item v-bind="scope.itemProps">
              <q-item-section>
                <span class="text-bold">{{ scope.opt.value }}</span>
                <span class="text-grey-7"> {{ scope.opt.label }}</span>
              </q-item-section>
            </q-item>
          </template>
        </q-select>
      </div>

      <div class="col-6">
        <div class="text-weight-medium q-mb-xs">Hacia</div>
        <q-select
          v-model="to"
          :options="currencyOptions"
          outlined
          emit-value
          map-options
          option-label="label"
          option-value="value"
          :display-value="formatDisplay(to)"
        >
          <template v-slot:option="scope">
            <q-item v-bind="scope.itemProps">
              <q-item-section>
                <span class="text-bold">{{ scope.opt.value }}</span>
                <span class="text-grey-7"> {{ scope.opt.label }}</span>
              </q-item-section>
            </q-item>
          </template>
        </q-select>
      </div>
    </div>

    <div class="row justify-center q-my-md">
      <q-btn round icon="swap_vert" @click="swapCurrencies" />
    </div>

    <q-btn label="Convertir" color="primary" class="full-width" @click="convertCurrency" />

    <div v-if="result" class="text-center text-subtitle1 q-mt-md">
      {{ result }}
    </div>

    <q-banner v-if="error" class="bg-red-1 text-red-9 q-mt-md">
      {{ error }}
    </q-banner>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const amount = ref(null)
const from = ref(null)
const to = ref(null)
const result = ref('')
const error = ref('')
const currencyOptions = ref([])

onMounted(async () => {
  try {
    const res = await axios.get('https://api.frankfurter.app/currencies')
    currencyOptions.value = Object.entries(res.data).map(([value, label]) => ({
      label,
      value,
    }))
  } catch {
    error.value = 'No se pudieron cargar las monedas.'
  }
})

const formatDisplay = (code) => {
  const match = currencyOptions.value.find((opt) => opt.value === code)
  return match ? `${match.value} ${match.label}` : ''
}

const convertCurrency = async () => {
  error.value = ''
  result.value = ''

  if (!amount.value || amount.value <= 0) {
    error.value = 'Por favor, ingresa un monto válido.'
    return
  }
  if (!from.value || !to.value) {
    error.value = 'Selecciona ambas monedas.'
    return
  }
  if (from.value === to.value) {
    result.value = `${amount.value} ${from.value} equivalen a ${amount.value} ${to.value}`
    return
  }

  try {
    const res = await axios.get(
      `https://api.frankfurter.app/latest?amount=${amount.value}&from=${from.value}&to=${to.value}`,
    )
    const value = res.data.rates[to.value]
    result.value = `${amount.value} ${from.value} equivalen a ${value.toFixed(2)} ${to.value}`
  } catch {
    error.value = 'Error al realizar la conversión.'
  }
}

const swapCurrencies = () => {
  ;[from.value, to.value] = [to.value, from.value]
}
</script>
