<script setup>
import { ref, reactive, watch, computed, onMounted } from "vue";
import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";
import Filtros from "./components/Filtros.vue";
import Modal from "./components/Modal.vue";
import Gasto from "./components/Gasto.vue";

import iconoGasto from "../src/assets/img/nuevo-gasto.svg";
const presupuesto = ref(0);
const disponible = ref(0);
const gastado = ref(0);
const filtro = ref("");

const modal = reactive({
  mostrar: false,
  animar: false,
});
const gasto = reactive({
  nombre: "",
  cantidad: "",
  categoria: "",
  id: null,
  fecha: Date.now(),
});
const gastos = ref([]);
watch(
  gastos,
  () => {
    const gastosStorage = localStorage.setItem(
      "gastos",
      JSON.stringify(gastos.value)
    );

    const total = gastos.value.reduce((acc, total) => acc + total.cantidad, 0);
    gastado.value = total;
    disponible.value = presupuesto.value - gastado.value;
  },
  {
    deep: true,
  }
);
watch(presupuesto, () => {
  localStorage.setItem("presupuesto", presupuesto.value);
});
onMounted(() => {
  const presupuestoStorage = localStorage.getItem("presupuesto");
  const gastosStorage = JSON.parse(localStorage.getItem("gastos"));
  if (presupuestoStorage) {
    presupuesto.value = Number(presupuestoStorage);
    disponible.value = Number(presupuestoStorage);
  }
  if (gastosStorage) {
    gastos.value = gastosStorage;
  }
});
const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad;
  disponible.value = cantidad;
};
const mostrarModal = () => {
  modal.mostrar = true;
  setTimeout(() => {
    modal.animar = true;
  }, 500);
};
const ocultarModal = () => {
  modal.animar = false;
  setTimeout(() => {
    modal.mostrar = false;
  }, 500);
  gasto.nombre = "";
  gasto.cantidad = "";
  gasto.fecha = Date.now();
  gasto.categoria = "";
  gasto.id = null;
};
const editarGasto = (id) => {
  mostrarModal();
  const gastoSeleccionado = gastos.value.filter(
    (gastoState) => gastoState.id === id
  )[0];
  Object.assign(gasto, gastoSeleccionado);
};
const eliminarGasto = (id) => {
  const gastoEliminar = gastos.value.filter((gasto) => gasto.id != id);
  gastos.value = gastoEliminar;
  alert("Eliminado Correctamente");
  setTimeout(() => {
    ocultarModal();
  }, 1000);
};
const gastosFiltrado = computed(() => {
  if (filtro.value) {
    return gastos.value.filter((gasto) => gasto.categoria === filtro.value);
  }
  return gastos.value;
});
const resetearApp = () => {
  if (confirm("Estás Seguro")) {
    localStorage.clear();
    presupuesto.value = 0;
    disponible.value = 0;
    gasto.nombre = "";
    gasto.cantidad = "";
    gasto.fecha = Date.now();
    gasto.id = null;
    gasto.categoria = "";
  }
};
</script>

<template>
  <div :class="{ fijar: modal.mostrar }">
    <header>
      <h1>Control Gastos</h1>
      <div class="contenedor contenedor-header sombra">
        <Presupuesto
          @definir-presupuesto="definirPresupuesto"
          v-if="presupuesto === 0"
        />

        <ControlPresupuesto
          v-else
          :presupuesto="presupuesto"
          :disponible="disponible"
          :gastado="gastado"
          @resetear-app="resetearApp"
        />
      </div>
    </header>
    <main v-if="presupuesto > 0">
      <Filtros v-model:filtro="filtro" />
      <div class="listado-gastos contenedor">
        <h2>{{ gastosFiltrado.length ? "Gastos" : "No hay Gastos" }}</h2>
        <Gasto
          :gasto="gasto"
          v-for="gasto in gastosFiltrado"
          @editar-gasto="editarGasto"
        />
      </div>
      <div class="crear-gasto">
        <img :src="iconoGasto" alt="icono Gasto" @click="mostrarModal" />
      </div>
      <Modal
        v-if="modal.mostrar"
        @ocultar-modal="ocultarModal"
        :modal="modal"
        :gasto="gasto"
        :gastos="gastos"
        :presupuesto="presupuesto"
        :disponible="disponible"
        @eliminar-gasto="eliminarGasto"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"
      />
    </main>
  </div>
</template>

<style>
:root {
  --azul: #3b82f6;
  --blanco: #fff;
  --negro: #000;
  --gris: #94a3b8;
  --gris-claro: #f5f5f5;
  --gris-oscuro: #64748b;
}
html {
  box-sizing: border-box;
  font-size: 62.5%;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--gris-claro);
}
h1 {
  font-size: 4rem;
}
h2 {
  font-size: 3rem;
}
header {
  background-color: var(--azul);
}
header h1 {
  padding: 3rem 0;
  margin: 0;
  color: var(--blanco);
  text-align: center;
}
.contenedor {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}
.contenedor-header {
  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;
}
.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 5rem;
}
.crear-gasto {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
}
.crear-gasto img {
  width: 5rem;
  cursor: pointer;
}
.listado-gastos {
  margin-top: 10rem;
}
.listado-gastos h2 {
  text-transform: uppercase;
  text-align: center;
  font-weight: 900;
  color: var(--gris-oscuro);
}
.fijar {
  overflow: hidden;
  height: 100vh;
}
</style>
