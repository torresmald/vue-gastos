<script setup>
import { ref } from "vue";
import {uid} from 'uid';

import Alerta from "./Alerta.vue";
import cerrarModal from "../assets/img/cerrar.svg";

defineEmits([
  "ocultar-modal",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
]);
const props = defineProps({
  modal: {
    type: Object,
    required: true,
  },
  gasto: {
    nombre: String,
    cantidad: String,
    categoria: String,
    id: String,
    required: true,
  },
  gastos: {
    type: Array,
    required: true
  }
});
const error = ref("");
const categorias = ["gastos", "casa", "suscripciones", "salud", "comida", "ahorro", "ocio"];

const agregarGasto = () => {
  if (Object.values(props.gasto).includes("")) {
    error.value = "Todos los Campos son Obligatorios";
    setTimeout(() => {
      error.value = "";
    }, 2000);
    return;
  }
  if (props.gasto.cantidad <= 0) {
    error.value = "Cantidad no Válida";
    setTimeout(() => {
      error.value = "";
    }, 2000);
    return;
  }
  props.gasto.id = uid();
  props.gastos.push({...props.gasto});
 
  setTimeout(() => {
    props.gasto.nombre = ''
    props.gasto.cantidad = 0;
    props.gasto.categoria= ''
    props.modal.mostrar = false;
  }, 2000);
};
</script>
<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img
        :src="cerrarModal"
        alt="cerrar Modal"
        @click="$emit('ocultar-modal')"
      />
    </div>

    <div
      class="contenedor contenedor-formulario"
      :class="['props.modal.animar' ? 'animar' : 'cerrar']"
    >
      <form @submit.prevent="agregarGasto" class="nuevo-gasto">
        <legend>Añadir Gasto</legend>
        <Alerta v-if="error">
          {{ error }}
        </Alerta>
        <div class="campo">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            placeholder="Nombre del Gasto"
            :value="props.gasto.nombre"
            @input="$emit('update:nombre', $event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="cantidad">Cantidad</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Cantidad del Gasto"
            :value="props.gasto.cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>
        <div class="campo">
          <label for="categoria">Categoría</label>
          <select
            id="categoria"
            :value="props.gasto.categoria"
            @input="$emit('update:categoria', $event.target.value)"
          >
            <option value="">--Selecciona--</option>
            <option v-for="categoria in categorias" :value="categoria">
              {{ categoria }}
            </option>
          </select>
        </div>
        <input type="submit" value="Añadir Gasto" />
      </form>
    </div>
  </div>
</template>
<style scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 /0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.cerrar-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}
.cerrar-modal img {
  width: 3rem;
  cursor: pointer;
}
.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}
.contenedor-formulario.animar {
  opacity: 1;
}
.contenedor-formulario.cerrar {
  opacity: 0;
}
.nuevo-gasto {
  margin: 10rem auto 0 auto;
  display: grid;
  gap: 2rem;
}
.campo {
  display: grid;
  gap: 2rem;
}
.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 3rem;
  font-weight: 700;
}
.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  border: none;
  font-size: 2.2rem;
  padding: 1rem;
}
.nuevo-gasto label {
  color: var(--blanco);
  font-size: 3rem;
}
.nuevo-gasto input[type="submit"] {
  margin: 2rem 0;
  background-color: var(--azul);
  color: var(--blanco);
  font-weight: 700;
  cursor: pointer;
}
</style>
