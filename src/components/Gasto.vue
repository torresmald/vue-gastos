<script setup>
import { formatearDinero, formatearFecha } from '../helpers';

const props = defineProps({
  gasto: {
    type: Object,
    required: true,
  },
});
</script>
<template>
  <div class="gasto sombra">
    <div class="contenido">
        <div class="detalles">
            <p class="categoria">{{ gasto.categoria }}</p>
            <p class="nombre">{{ gasto.nombe }}</p>
            <p class="fecha">{{ formatearFecha(gasto.fecha) }}</p>

        </div>
    </div>
    <p class="cantidad">{{ formatearDinero(gasto.cantidad) }}</p>
  </div>
</template>

<style scoped>

</style>
