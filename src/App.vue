<template>
  <div id="app">
    <img src="./assets/cronometro.png" />
    <a class="timer">{{ numero }}</a>
    <div class="areaBtn">
      <button class="botao" @click="comecar">{{ botao }}</button>
      <button class="botao" @click="limpar">LIMPAR</button>
    </div>
    <div class="list" v-show="historico.length > 0">
      <ul>
        <li v-for="item in historico" :key="item">
          VOCÊ FEZ UMA PAUSA EM: {{ item }}
        </li>
      </ul>
      <button @click="historico = []">Limpar histórico</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      numero: 0,
      botao: "COMEÇAR",
      timer: null,
      ss: 0,
      mm: 0,
      hh: 0,
      historico: [],
    };
  },
  methods: {
    comecar() {
      if (this.timer != null) {
        //tem algo rodando no timer
        clearInterval(this.timer);
        this.timer = null;
        this.botao = "COMEÇAR";
        if (this.ss !== 0) {
          this.historico.push(this.numero);
        }
      } else {
        //timer está zerado ou parado
        this.timer = setInterval(() => {
          this.rodarTimer();
        }, 1000);
        this.botao = "PAUSAR";
      }
    },
    limpar() {
      if (this.timer != null) {
        clearInterval(this.timer);
        this.timer = null;
      }
      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
      this.numero = 0;
      this.botao = "COMEÇAR";
      this.historico = [];
    },

    rodarTimer() {
      this.ss++;

      if (this.ss == 59) {
        // 59 segundos
        this.ss = 0;
        this.mm++;
      }

      if (this.mm == 59) {
        //59 minutos
        this.mm = 0;
        this.hh++;
      }

      let format =
        (this.hh < 10 ? "0" + this.hh : this.hh) +
        ":" +
        (this.mm < 10 ? "0" + this.mm : this.mm) +
        ":" +
        (this.ss < 10 ? "0" + this.ss : this.ss);

      return (this.numero = format);
    },
  },
};
</script>

<style>
#app {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
img {
  width: 420px;
  height: 420px;
  padding-top: 100px;
}
.timer {
  color: white;
  font-size: 70px;
  margin-top: -250px;
}
.areaBtn {
  margin-top: 200px;
  display: flex;
}
.botao {
  -webkit-user-select: none;
  -moz-user-select: none;
  width: 150px;
  background-color: #ffffff;
  font-size: 20px;
  border: 0;
  border-radius: 5px;
  text-align: center;
  margin-left: 15px;
  margin-right: 15px;
  padding: 6px;
  cursor: pointer;
}
.botao:hover {
  opacity: 0.6;
  transition: all 0.5s;
}
ul {
  text-align: center;
  padding: 0;
}
ul li {
  margin-top: 4px;
  padding: 15px;
  background-color: #707070;
  list-style: none;
  color: #ffffff;
  border-radius: 10px;
}
.list button {
  cursor: pointer;
  border: 0;
  background-color: #ffffff;
  padding: 8px;
  border-radius: 5px;
  margin-bottom: 12px;
}
</style>
