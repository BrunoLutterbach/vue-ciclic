<template>
  <div class="d-flex align-items-center" style="height: 100vh">
    <div
      class="container border border-2 border-dark rounded"
      style="width: 500px"
    >
      <p class="fs-2 fw-bold text-center">Ciclic</p>
      <div class="alert alert-danger text-center" role="alert" v-if="alerta">
        Ocorreu um erro, verifique as informações.
      </div>
      <hr />
      <h3 class="mb-3">Simulador</h3>
      <form>
        <div>
          <div class="mb-3 row">
            <label type="text" class="col-3 col-form-label">Nome</label>
            <div class="col">
              <input type="text" class="form-control" v-model="dados.nome" />
            </div>
          </div>

          <div class="mb-3 row">
            <label type="text" class="col-3 col-form-label">Mensalidade</label>
            <div class="col">
              <input
                type="number"
                class="form-control"
                placeholder="Valor depositado por mês"
                v-model="dados.mensalidade"
              />
            </div>
          </div>

          <div class="mb-3 row">
            <label type="text" class="col-3 col-form-label">Tempo</label>
            <div class="col">
              <select class="form-select mb-3" v-model="dados.tempo">
                <option selected>Selecionar</option>
                <option value="1">1 Mês</option>
                <option value="2">2 Meses</option>
                <option value="3">3 Meses</option>
                <option value="4">4 Meses</option>
                <option value="5">5 Meses</option>
                <option value="6">6 Meses</option>
                <option value="7">7 Meses</option>
                <option value="8">8 Meses</option>
                <option value="9">9 Meses</option>
                <option value="10">10 Meses</option>
                <option value="11">11 Meses</option>
                <option value="12">12 Meses</option>
              </select>
            </div>
          </div>
          <div class="d-flex justify-content-center">
            <button
              type="button"
              class="btn btn-success mb-2 shadow"
              @click="Simular()"
            >
              Simular
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Calcular",
  data: () => ({
    dados: {
      nome: "",
      mensalidade: "",
      tempo: "",
      resultado: "",
    },
    infos: {},
    formData: "",
    alerta: false,
  }),
  methods: {
    Simular() {
      if (this.validarFormulario()) {
        this.formData = {
          expr: `${this.dados.mensalidade} * (((1 + 0.00517) ^ ${this.dados.tempo} - 1) / 0.00517)`,
        };
        axios
          .post("https://api.mathjs.org/v4/", this.formData)
          .then(
            (response) =>
              (this.dados.resultado = response.data.result.toLocaleString(
                "pt-br",
                { style: "currency", currency: "BRL" }
              ))
          )
          .catch((error) => console.log(error));

        this.emitter.emit("Simular", "Resultado");
        setTimeout(() => {
          this.emitter.emit("Dados", this.dados);
        }, 1000);
      } else {
        this.alerta = true;
        setTimeout(() => {
          this.alerta = false;
        }, 5000);
      }
    },

    validarFormulario() {
      let valido = true;

      if (this.dados.nome === "") valido = false;
      if (this.dados.mensalidade === "") valido = false;
      if (this.dados.tempo === "") valido = false;

      return valido;
    },
    resetaFormulario() {
      this.dados.nome = "";
      this.dados.mensalidade = "";
      this.dados.tempo = "";
    },
  },
    activated() {
    this.resetaFormulario()
  },
};
</script>
