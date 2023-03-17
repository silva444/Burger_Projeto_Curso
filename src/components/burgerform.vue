<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <form id="burger-form" @submit="createburger">
      <div class="input-container">
        <label for="nome">Nome do Cliente </label>
        <input
          type="text"
          id="nome"
          name="name"
          v-model="nome"
          placeholder="digite o nome"
        />
      </div>
      <div class="input-container">
        <label for="pao">Escolha o Pão: </label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o Pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a Carnne: </label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de Carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-tittle" for="opcionais"
          >Selecione os opcionais:
        </label>
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
          <!-- interpolação de dados -->
        </div>
      </div>
      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar Meu Burger" />
      </div>
    </form>
  </div>
</template>

<script>
import Message from "./message.vue";
export default {
  components: {
    Message,
  },
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: "",
      pao: "",
      carne: "",
      opcionais: [],
      status: "",
      msg: null,
    };
  },
  methods: {
    async getIngredientes() {
      // vão ser buscados por esssas rotas;
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();
      //console.log(data)
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createburger(e) {
      e.preventDefault();
      console.log("Criou o Hamburguer");
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      console.log(data);

      const dataJson = JSON.stringify(data); // transsformando o dado em texto para o navegador en

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      this.msg = `pedido numero ${res.id} realizado com sucesso`;
      setTimeout(() => (this.msg = ""), 3000);
      console.log(res);
      this.nome = "";
      this.pao = "";
      this.carne = "";
      this.opcionais = "";
    },
  },
  mounted() {
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}
.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}
input,
select {
  padding: 5px 10px;
  width: 300px;
}
#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#opcionais-title {
  width: 100%;
}
.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto; /* para fica com as larguras que eles tem e não as que eles herdaram */
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>