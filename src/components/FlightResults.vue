<template>
  <div class="results-card" v-if="erro || voos.length || loading">
    <div class="status-bar">
      <span class="results-count">{{ voos.length }} voo(s) encontrado(s)</span>
      <div v-if="loading" class="loading-spinner"></div>
    </div>

    <div v-if="erro" class="error-message">{{ erro }}</div>

    <div v-if="voos.length" class="table-container">
      <table>
        <thead>
          <tr>
            <th>Data</th>
            <th>Aeronave</th>
            <th>Origem</th>
            <th>Destino</th>
            <th>Partida</th>
            <th>Chegada</th>
            <th>Duração</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="voo in voos" :key="voo.date + voo.aircraftId">
            <td>{{ voo.date || '-' }}</td>
            <td>{{ voo.aircraftId || '-' }}</td>
            <td>{{ voo.origin || '-' }}</td>
            <td>{{ voo.destination || '-' }}</td>
            <td>{{ voo.departure || '-' }}</td>
            <td>{{ voo.arrival || '-' }}</td>
            <td>{{ voo.duration || '-' }}</td>
            <td><span class="status-badge" :class="statusClass(voo.status)">{{ voo.status || '-' }}</span></td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="empty-state" v-if="!voos.length && !erro && !loading">
      <svg viewBox="0 0 24 24" fill="currentColor">
        <path d="M21,16V14L13,9V7A1,1 0 0,0 12,6A1,1 0 0,0 11,7V9L3,14V16L11,13.5V19L8,20.5V22L12,21L16,22V20.5L13,19V13.5L21,16Z" />
      </svg>
      <h3>Nenhum voo encontrado</h3>
      <p>Tente ajustar os filtros de busca</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  voos: Array,
  loading: Boolean,
  erro: String
});

function statusClass(status) {
  if (!status) return '';
  const s = status.toLowerCase();
  if (s.includes('completed') || s.includes('arrived')) return 'status-completed';
  if (s.includes('active') || s.includes('en route')) return 'status-active';
  if (s.includes('cancelled') || s.includes('canceled')) return 'status-cancelled';
  return '';
}
</script>
