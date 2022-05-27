<template>

  <div class="app">
    <!-- ========== Header ========== -->
    <header class="font-black text-6xl text-stone-400 pt-20 justify-center flex">
      <h1>THE<strong class="text-stone-800">COIN</strong>APP</h1>
    </header>
    <!-- ========== Main ========== -->
    <main>
      <form class="pt-10 justify-center flex search-bar h-full" @submit.prevent="AddCoin">
        <input placeholder="Search for currency to add..."
          class="bg-zinc-100 shadow-lg max-w-2xl w-3/5 py-3 pl-5 rounded-lg" type="search" required v-model="input">
      </form>
      <ul class="justify-center cards-container gap-10 grid py-20 container mx-auto">
        <template v-for="coin in this.coinList.slice(0, 10)">
          <CoinCard :key="coin.asset_id" :coin="coin" v-if="isNaN(parseFloat(coin.price_usd).toFixed(4)) == false" />
        </template>
        <template v-if="newCoins.length > 0">
          <CoinCard v-for="coin in newCoins" :key="coin.name" :coin="coin" />
        </template>
      </ul>
    </main>
  </div>

</template>

<script>
import { ref } from "vue";
import axios from 'axios'
import CoinCard from "./components/CoinCard.vue";

export default {
  name: "App",

  setup() {
    const coinList = ref([])
    const input = ref("")
    const newCoins = ref([])

    const AddCoin = () => {
      let obj = {}

      coinList.value.forEach(element => {
        if (element.name.toUpperCase() == input.value.toUpperCase() || element.asset_id == input.value.toUpperCase()) {
          newCoins.value.push(element)
          obj = element
        }
      })
      if (obj.name == undefined) {
        alert("Moeda Invalida")
      }
    }
    return {
      AddCoin,
      input,
      newCoins,
      coinList,
    }
  },

  computed() {

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
          })
        });
    }

    getCoins()


  },
  components: { CoinCard }
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
  background-color: #313131;
}

.cards-container {
  justify-items: center;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
}
</style>
