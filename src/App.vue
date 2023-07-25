<script setup>
import { ref } from "vue";
import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";

const presupuesto = ref(0);
const definirPresupuesto = cantidad =>{
  presupuesto.value = cantidad;
}
</script>

<template>
  <div>
    <header>
      <h1>Desde VUE</h1>
      <div class="contenedor contenedor-header sombra">
        <Presupuesto @definir-presupuesto="definirPresupuesto" v-if="presupuesto === 0"/>

        <ControlPresupuesto />
      </div>
    </header>
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
.contenedor{
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}
.contenedor-header{
  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;
}
.sombra{
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 5rem;
}
</style>
