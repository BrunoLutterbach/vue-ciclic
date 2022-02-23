<template>
  <div class="d-flex align-items-center" style="height: 100vh">
    <div
      class="container border border-2 border-dark rounded text-center"
      style="width: 500px; height: 358px"
    >
      <p class="fs-2 fw-bold text-center">Ciclic</p>
      <hr />
      <h4 class="pt-4">Olá {{ dados.nome }}</h4>
      <h4>
        Juntando 
        {{
          parseInt(dados.mensalidade).toLocaleString("pt-br", {
            style: "currency",
            currency: "BRL",
          })
        }}
        todo mês,
      </h4>
      <h4>
        Você terá 
        {{
          parseInt(dados.resultado).toLocaleString("pt-br", {
            style: "currency",
            currency: "BRL",
          })
        }}
      </h4>
      <h4>Em {{ dados.tempo }} meses</h4>
      <div class="d-flex justify-content-center pt-3">
        <button
          type="button"
          class="btn btn-success mb-2 shadow"
          @click="Simular()"
        >
          Simular Novamente
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Resultado",
  data: () => ({
    dados: {},
  }),
  methods: {
    Simular() {
      this.emitter.emit("Simular", "Calcular");
    },
  },

  mounted() {
    this.emitter.on("Dados", (p) => {
      this.dados = p;
    });
  },
};
</script>
