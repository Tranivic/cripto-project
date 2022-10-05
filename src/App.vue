<template>

  <div class="app">
    <!-- ========== Header ========== -->
    <header class="font-black md:text-6xl sm:text-5xl text-stone-400 pt-20 justify-center flex">
      <h1>THE<strong class="text-stone-800">CRIPTO</strong>DATA</h1>
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
import axios from 'axios';

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

    const getCoins = () => {
      let formatedCoins = [];
      axios.get(`https://rest.coinapi.io/v1/assets/?apikey=836C99F4-29ED-4AFC-AB22-A4DB3678C94B`)
        .then(response => {
          let myTarget = JSON.parse(JSON.stringify(response.data))
          myTarget.forEach(element => {
            if (element.id_icon && element.price_usd) {
              formatedCoins.push(element);
            }
          })
          coinList.value = formatedCoins;
          formatCoins();
        });
    }
    //Function to formated coins array pulled from api database
    const formatCoins = () => {
      coinList.value.sort(function (a, b) {
        return a.name < b.name ? -1 : a.name > b.name ? 1 : 0;
      });
      coinList.value.forEach((element) => {
        element.name[0].toUpperCase() + element.name.slice(1).toLowerCase();
        element.id_icon = element.id_icon.replace(/-/g, "");
      });
    }

    //Function att coins prices every 5 minutes
    const attPrices = () => {
      // getCoins();
      // let obj = {};
      // if (newCoins.value.length > 0) {
      //   newCoins.value.forEach((element) => {
      //     pushNewCoin(obj, element)
      //   });
      //   console.log("Atualizei");
      // }
    };
    setInterval(attPrices, 5000);
    // Function to push coin to array list
    const pushNewCoin = () => {
      let counter = 0
      coinList.value.forEach(element => {
        if (element.name.toUpperCase() == input.value.toUpperCase() || element.asset_id == input.value.toUpperCase()) {
          counter = counter + 1;
          newCoins.value.push(element);
          dropList.value = [];
        }
      })
      if (counter == 0) {
        hide.value = true;
        counter = 0;
      } else {
        hide.value = false;
        counter = 0;
      }
    };

    // Fuction to add coin
    const addCoin = () => {
      if (dropList.value.length === 1) {
        input.value = dropList.value[0].name;
      }
      pushNewCoin();
    };

    //Function to show autocomplete list
    const showDrop = () => {
      var inp = input.value.toUpperCase();
      dropList.value = [];

      if (input.value.length > 1) {
        dropList.value = [];
        coinList.value.forEach(element => {
          if (element.name.toUpperCase().startsWith(inp)) {
            dropList.value.push(element);
          }
        });
      }
    };

    // Function to push coin to array when autocomplete item is clicked
    const dropClicked = (dropSelected) => {
      input.value = dropSelected.name;
      dropList.value = [];
      addCoin();
    };

    onMounted(() => {
      getCoins();
      formatCoins();
    });

    return {
      addCoin,
      showDrop,
      dropClicked,
      getCoins,
      attPrices,
      formatCoins,
      dropList,
      input,
      hide,
      newCoins,
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
