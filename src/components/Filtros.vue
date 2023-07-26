<script setup>
defineEmits(['update:filtro']);
defineProps(['filtro'])
const categorias = [
  "gastos",
  "casa",
  "suscripciones",
  "salud",
  "comida",
  "ahorro",
  "ocio",
];
</script>

<template>
  <div class="filtros sombra contenedor">
    <form>
      <div class="campo">
        <label for="filtro">Filtrar Gastos</label>
        <select
          id="filtro"
          :value="filtro"
          @input="$emit('update:filtro', $event.target.value)"
        >
          <option value="">--Selecciona--</option>
          <option v-for="categoria in categorias" :value="categoria">
            {{ categoria }}
          </option>
        </select>
      </div>
    </form>
  </div>
</template>

<style scoped>
.filtros{
    margin-top: 10rem;
}
.filtros .campo{
    display: flex;
    align-items: center;
    gap: 2rem;
}
.filtros label{
    font-size: 3rem;
    font-weight: 900;
    color: var(--gris-oscuro);
}
.filtros select{
    flex: 1;
    padding: 1rem;
    border: none;
    border-radius: 1rem;
    background-color: var(--gris-claro);
    text-align: center;
}
</style>
