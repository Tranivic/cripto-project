<template>

  <div class="app">
    <!-- ========== Header ========== -->
    <header class="font-black text-6xl text-stone-400 pt-20 justify-center flex">
      <h1>THE<strong class="text-stone-800">COIN</strong>APP</h1>
    </header>
    <!-- ========== Main ========== -->
    <main>
      <form class="pt-10 flex search-bar h-full flex-col text-center items-center" @submit.prevent="AddCoin">
        <input placeholder="Search for currency to add..."
          class="bg-zinc-100 shadow-lg max-w-2xl w-3/5 py-3 pl-5 rounded-lg" type="search" required v-model="input"
          v-on:input="showDrop">
        <DropDown v-show="input.length" :drop-list="dropList" />
      </form>
      <div class="notFindMsg flex justify-center">
        <h1 v-show="this.hide == true" class="max-w-2xl w-3/5 text-red-500">Coin not found...</h1>
      </div>
      <ul class="justify-center cards-container gap-10 grid py-20 container mx-auto">
        <template v-if="newCoins.length">
          <CoinCard v-for="coin in newCoins" :key="coin.name" :coin="coin" />
        </template>
      </ul>
    </main>
  </div>

</template>

<script>
import { ref } from "vue";
import axios from 'axios';
import CoinCard from "./components/CoinCard.vue";
import DropDown from "./components/DropDown.vue";

export default {
  name: "App",
  data() {
    return {
      yourCurrency: "USD",
    };
  },
  setup() {
    const coinList = ref([]);
    const newCoins = ref([]);
    const dropList = ref([]);
    const input = ref("");
    const hide = ref(false);

    const AddCoin = () => {
      let obj = {}

      coinList.value.forEach(element => {
        if (element.name.toUpperCase() == input.value.toUpperCase() || element.asset_id == input.value.toUpperCase()) {
          newCoins.value.push(element)
          obj = element
        }
      })
      if (obj.name == undefined) {
        hide.value = true
      } else {
        hide.value = false
      }
    };
    const showDrop = () => {

      var inp = input.value.toUpperCase()
      dropList.value = []

      coinList.value.forEach(element => {
        if (element.name.toUpperCase().startsWith(inp)) {
          dropList.value.push(element)
          console.log(dropList)
        }
      })
    }
    return {
      AddCoin,
      showDrop,
      dropList,
      input,
      hide,
      newCoins,
      coinList,
    }
  },


  mounted() {
    const getCoins = () => {
      axios.get("https://rest.coinapi.io/v1/assets/?apikey=836C99F4-29ED-4AFC-AB22-A4DB3678C94B")
        .then(response => {
          let myTarget = JSON.parse(JSON.stringify(response.data))
          this.coinList = myTarget
          this.coinList.forEach(element => {
            if (element.id_icon != undefined) {
              element.id_icon = element.id_icon.replace(/-/g, "")
            }
            //First itens of the list to display on screen
            if (element.name === "Bitcoin" || element.name === "Ethereum" || element.name === "Ethereum Classic" || element.name === "Rotharium") {
              this.newCoins.push(element)
            }
          })
        });
    }
    getCoins()


  },
  components: { CoinCard, DropDown }
}

</script>

<style>
/* ================= Reset CSS ================*/
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: 'Fira Sans', sans-serif;
}

a {
  text-decoration: none;
}

/*=============================================*/
input {
  transition: 0.2s;
}

input:focus,
input:valid {
  color: #fff;
  background-color: rgb(41 37 36);
  outline: none !important;
}

.cards-container {
  justify-items: center;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
}
</style>
