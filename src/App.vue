<template>
  <div class="container">
    <header class="header">
      <h1>✈️ FlightInfo BR</h1>
      <p>Consulta de voos offshore em tempo real</p>
    </header>

    <FlightSearch @onSearch="buscarVoos" />
    <FlightResults :voos="voos" :loading="loading" :erro="erro" />

    <footer class="footer">
      <p>&copy; 2025 FlightInfo BR - Dados via FlightAware API</p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import FlightSearch from './components/FlightSearch.vue';
import FlightResults from './components/FlightResults.vue';

const voos = ref([]);
const loading = ref(false);
const erro = ref(null);

async function buscarVoos({ airport, aircraft }) {
  erro.value = null;
  voos.value = [];

  if (!airport && !aircraft) {
    erro.value = 'Informe pelo menos um filtro de busca';
    return;
  }

  loading.value = true;

  try {
    const response = await fetch(
      `https://flightinfo.onrender.com/flights?${new URLSearchParams({ airport, aircraft })}`
    );
    const json = await response.json();
    voos.value = json.data || [];
  } catch (e) {
    erro.value = e.message;
  } finally {
    loading.value = false;
  }
}
</script>
