<template >
    <li class="CoinCard coin-card py-5 bg-zinc-100 rounded-lg shadow-lg w-60">
        <div class="card-top flex justify-center items-center">
            <div class="currency-logo pr-3 rounded-full">
                <img src="https://upload.wikimedia.org/wikipedia/commons/4/46/Bitcoin.svg" alt="Currency logo">
            </div>
            <h1 class="text-2xl">{{ coin.name }}</h1>
            <h2 class="pl-2 text-stone-400">{{ coin.asset_id }}</h2>
        </div>
        <div class="card-content flex justify-center text-center">
            <h1 class="text-2xl">${{ parseFloat(coin.price_usd).toFixed(4) }}</h1>
        </div>
    </li>
</template>

<script>
export default {
    props: ['coin'],
    data() {
        return {
            numero: "oi",
        }
    },

}
</script>

<style>
.coin-card {
    transition: 0.2s ease-in-out;
}

.coin-card:hover {
    cursor: pointer;
    color: #fff;
    background-color: #313131;
    transform: scale(110%);
}
</style>