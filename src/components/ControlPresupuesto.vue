<script setup>
import {computed} from 'vue';
import CircleProgress from 'vue3-circle-progress';
import "vue3-circle-progress/dist/circle-progress.css"; 
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
defineEmits(['resetear-app']);

const porcentaje = computed(() => {
   return parseInt(((props.presupuesto - props.disponible) / props.presupuesto) * 100);
});
</script>
<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <p class="porcentaje">{{ porcentaje }}%</p>
      <CircleProgress 
        :percent="porcentaje"
      />
    </div>
    <div class="contenedor-presupuesto">
      <button class="reset-app" @click="$emit('resetear-app')">Resetear App</button>
      <p>
        Presupuesto: <span> {{ formatearDinero( props.presupuesto) }}</span>
      </p>
      <p>Disponible: <span> {{ formatearDinero( props.disponible) }}</span></p>
      <p>Gastado: <span> {{ formatearDinero(props.gastado) }} </span></p>
    </div>
  </div>
</template>

<style scoped>
.contenedor-grafico{
  position: relative;
}
.porcentaje{
  position: absolute;
  margin: auto;
  top: calc(50% - 1.5rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 3rem;
  font-weight: 900;
  color: var(--gris-oscuro);
  
}
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
