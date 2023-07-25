<script setup>
import imagen from "../assets/img/grafico.jpg";
import { formatearDinero} from '../helpers/index.js';
const props = defineProps({
  presupuesto: {
    type: Number,
    required: true,
  },
  disponible: {
    type: Number,
    required: true
  },
  gastado:{
    type: Number,
    required: true
  }
});
</script>
<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <img :src="imagen" alt="" />
    </div>
    <div class="contenedor-presupuesto">
      <button class="reset-app">Resetear App</button>
      <p>
        Presupuesto: <span> {{ formatearDinero( props.presupuesto) }}</span>
      </p>
      <p>Disponible: <span> {{ formatearDinero( props.disponible) }}</span></p>
      <p>Gastado: <span> {{ formatearDinero(props.gastado) }} </span></p>
    </div>
  </div>
</template>

<style scoped>
.dos-columnas {
  display: flex;
  flex-direction: column;
}
.dos-columnas > :first-child {
  margin-bottom: 3rem;
}
@media screen and (min-width: 768px) {
  .dos-columnas {
    flex-direction: row;
    gap: 4rem;
    align-items: center;
  }
  .dos-columnas > :first-child {
    margin-bottom: 0;
  }
}
.reset-app {
  background-color: #d12121;
  border: none;
  padding: 1rem;
  width: 100%;
  color: var(--blanco);
  font-weight: 900;
  text-transform: uppercase;
  border-radius: 1rem;
  transition: background-color 300ms ease;
}
.reset-app:hover {
  cursor: pointer;
  background-color: #8a0303;
}
.contenedor-presupuesto {
  width: 100%;
}
.contenedor-presupuesto p {
  font-weight: 900;
  font-size: 2.4rem;
  text-align: center;
  color: var(--azul);
}
@media screen and (min-width: 768px) {
  .contenedor-presupuesto p {
    text-align: left;
  }
}
.contenedor-presupuesto span {
  color: var(--gris-oscuro);
}
</style>
