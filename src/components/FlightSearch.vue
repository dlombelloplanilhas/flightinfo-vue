<template>
  <div class="search-card">
    <form class="search-form" @submit.prevent="onSubmit">
      <div class="input-group">
        <label for="airport">Aeroporto</label>
        <input v-model="airport" id="airport" maxlength="4" placeholder="Ex: SBME, SBFS" />
      </div>
      <div class="input-group">
        <label for="aircraft">Aeronave</label>
        <input v-model="aircraft" id="aircraft" placeholder="Ex: OHA, PROHA" />
      </div>
      <button class="search-btn" type="submit">Buscar</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const emit = defineEmits(['onSearch']);
const airport = ref('');
const aircraft = ref('');

function onSubmit() {
  emit('onSearch', { airport: airport.value.toUpperCase(), aircraft: aircraft.value.toUpperCase() });
}
</script>
