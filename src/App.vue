<template>

  <div class="app">
    <!-- ========== Header ========== -->
    <header class="font-black text-6xl text-stone-400 pt-20 justify-center flex">
      <h1>THE<strong class="text-stone-800">COIN</strong>APP</h1>
    </header>
    <!-- ========== Main ========== -->
    <main>
      <form class="pt-10 flex search-bar h-full flex-col text-center items-center" @submit.prevent="addCoin">
        <input placeholder="Search for currency to add..."
          class="bg-zinc-100 shadow-lg max-w-2xl w-3/5 py-3 pl-5 rounded-lg" type="search" required v-model="input"
          @input="showDrop">
        <ul v-show="dropList.length"
          class="drop-list bg-stone-800 text-white shadow-lg max-w-2xl w-3/5 rounded-lg rounded-t-none text-left">
          <DropDown v-for="drop in dropList" :key="drop.asset_id" :drop="drop" @drop-clicked="dropClicked" />
        </ul>
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
  <!-- Api para consultar BRL - http://economia.awesomeapi.com.br/json/last/USD-BRL -->
</template>

<script>
import { ref, onMounted } from "vue";
import CoinCard from "./components/CoinCard.vue";
import DropDown from "./components/DropDown.vue";
import json from './database/rest.coinapi.io.json'

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
    const pushedCoins = ref([]);
    const dropList = ref([]);
    const input = ref("");
    const hide = ref(false);

    //Function to get coins list from api database
    const getCoins = () => {
      let formatedCoins = [];
      coinList.value = json;
      coinList.value.forEach((element) => {
        if (element.id_icon && element.price_usd) {
          element.id_icon = element.id_icon.replace(/-/g, "");
          formatedCoins.push(element);
        }
      })
      coinList.value = formatedCoins;
    }

    //Function att coins prices every 5 minutes
    const attPrices = () => {
      getCoins();
      let obj = {};
      if (newCoins.value.length > 0) {
        newCoins.value.forEach((element) => {
          pushNewCoin(obj, element)
        });
        console.log("Atualizei");
      }
    };
    setInterval(attPrices, 5000);
    // Function to push coin to array list
    const pushNewCoin = (o, e) => {
      if (e.name.toUpperCase() == input.value.toUpperCase() || e.asset_id == input.value.toUpperCase()) {
        newCoins.value.push(e)
        pushedCoins.value.push(e)
        o = e
        dropList.value = []
        input.value = ""
        if (o.name == undefined) {
          hide.value = true
        } else {
          hide.value = false
        }
      }

    };
    // Fuction to add coin
    const addCoin = () => {
      let obj = {}
      if (dropList.value.length === 1) {
        input.value = dropList.value[0].name
      }
      coinList.value.forEach(element => {
        pushNewCoin(obj, element)
      })
    };

    //Function to show autocomplete list
    const showDrop = () => {
      var inp = input.value.toUpperCase()
      dropList.value = []

      if (input.value.length > 1) {
        dropList.value = []
        coinList.value.forEach(element => {
          if (element.name.toUpperCase().startsWith(inp)) {
            dropList.value.push(element)
          }
        });
      }
    };

    // Function to push coin to array when autocomplete item is clicked
    const dropClicked = (dropSelected) => {
      input.value = dropSelected.name
      dropList.value = []
      addCoin()
    };

    onMounted(() => {
      getCoins();
    });

    return {
      addCoin,
      showDrop,
      dropClicked,
      getCoins,
      attPrices,
      dropList,
      input,
      hide,
      newCoins,
      pushedCoins,
      coinList,
    }
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
