<template>
  <div class="alerta">
    <slot></slot>
  </div>
</template>

<style scoped>
.alerta {
  padding: 1rem;
  font-weight: 900;
  text-transform: uppercase;
  font-size: 1.6rem;
  text-align: center;
  background-color: #891c1c;
  border: 0.5rem solid #891c1c;
  color: var(--blanco);
  margin-bottom: 2rem;
  border-radius: 1rem;
}
</style>
