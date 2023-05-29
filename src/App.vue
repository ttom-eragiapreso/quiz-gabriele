<script>
  import { onMounted } from "vue";
  import { store } from "./data/store";

  export default {
    data() {
      return {
        store,
        lezione: 0,
        quantity: 0,
        domandeFinal: [],
        risposteFinal: []
      };
    },
    methods: {
      arrayShuffle(array) {
        array.sort(() => Math.random() - 0.5);
      },

      generaDomande() {
        this.domandeFinal = [];
        this.risposteFinal = [];
        const domandeInLezione = store.domande.filter(
          (d) => d.lezione == this.lezione
        );
        this.arrayShuffle(domandeInLezione);

        for (let i = 0; i < this.quantity; i++) {
          this.domandeFinal.push(domandeInLezione[i]);
          this.risposteFinal.push({
            risposta: false,
            corretta: false
          });
        }
      },

      controllaRisposte() {
        for (let i = 0; i < this.risposteFinal.length; i++) {
          if (this.risposteFinal[i].risposta == this.domandeFinal[i].risposta) {
            this.risposteFinal[i].corretta = true;
          }
        }
      }
    }
  };
</script>

<template>
  <select
    name="lezione"
    id="lezione"
    v-model="lezione">
    <option value="0">Scegli la lezione da cui vuoi generare le domande</option>
    <option
      v-for="n in 12"
      :value="n">
      {{ n }}
    </option>
  </select>
  <select
    name="quantità"
    id="quantità"
    v-model="quantity">
    <option value="0">Scegli quante domande vuoi generare</option>
    <option
      v-for="n in 30"
      :value="n">
      {{ n }}
    </option>
  </select>

  <button @click="generaDomande">Genera domande random</button>

  <div
    v-for="(domanda, index) in domandeFinal"
    :key="index"
    class="domanda">
    <p
      class="basic"
      :class="[risposteFinal[index].corretta ? 'sbagliata' : 'corretta']">
      {{ domanda.domanda }}
    </p>
    <div class="check">
      <label :for="'vero_' + index">Vero</label>
      <input
        type="checkbox"
        :value="true"
        v-model="risposteFinal[index].risposta"
        :id="'vero_' + index" />
    </div>
  </div>

  <button @click="controllaRisposte">Controlla le tue risposte</button>
</template>

<style scoped>
  .basic {
    color: black;
  }

  p {
    margin: 0;
    text-align: center;
  }
  .domanda {
    display: flex;
    padding: 1.3rem 0;
  }

  .check {
    display: flex;
    height: 100%;
    justify-content: center;
    align-items: center;
    padding: 0 1rem;
  }

  #vero {
    margin-right: 1rem;
  }

  .corretta {
    color: red;
  }

  .sbagliata {
    color: green;
  }
</style>
