<template >
    <li class="CoinCard coin-card py-5 bg-zinc-100 rounded-lg shadow-lg w-80">
        <div class="card-top flex justify-center items-center">
            <img :src="'https://s3.eu-central-1.amazonaws.com/bbxt-static-icons/type-id/png_512/' + coin.id_icon + '.png'"
                alt="Base logo">
            <h1 class="text-2xl">{{ coin.name }}</h1>
            <h2 class="pl-2 text-stone-400">{{ coin.asset_id }}</h2>
        </div>
        <div class="card-content flex justify-center text-center">
            <h1 v-if="coin.price_usd" class="text-2xl">${{ parseFloat(coin.price_usd).toFixed(2) }}</h1>
        </div>
    </li>
</template>

<script>
export default {
    props: ['coin'],
    data() {
        return {

        }
    },

}
</script>

<style>
.coin-card {
    transition: 0.2s ease-in-out;
}

img {
    width: 40px;
    padding-right: 1rem;
}

.coin-card:hover {
    cursor: pointer;
    color: #fff;
    background-color: rgb(41 37 36);
    transform: scale(110%);
}
</style>