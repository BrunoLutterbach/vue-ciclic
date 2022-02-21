<template>
  <keep-alive>
    <component :is="conteudo" />
  </keep-alive>
</template>

<script>
import Calcular from "./components/Calcular.vue";
import Resultado from "./components/Resultado.vue";

export default {
  components: { Calcular, Resultado },
  name: "App",
  data: () => ({
    conteudo: "Calcular",
  }),
  mounted() {
    this.emitter.on("Simular", (p) => {
      this.conteudo = p;
    });
  },
};
</script>

<style></style>
