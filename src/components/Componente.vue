<template>
  <div class="containerC">
    <img :src="imagen" alt="texto" />
    <p>{{ texto }}</p>
  </div>
</template>

<script>
export default {
  props: {
    imagen: {
      type: String, // Tipo de dato esperado
      required: false,
    },
    texto: {
      type: String, // Tipo de dato esperado
      required: false,
    },
  },
};
</script>

<style scoped>
.containerC {
  display: flex;
  flex-direction: column;
  align-items: center;
}

img {
  width: 200px;
  height: 200px;
}
</style>