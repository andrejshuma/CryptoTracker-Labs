<script setup>

import { computed, ref } from "vue";
import { useCoinsStore } from "../store/coinsStore";

const coinStore = useCoinsStore();

const sortBy = ref(null); // null | 'change' | 'volume'

const displayedCoins = computed(() => {
    const coins = [...coinStore.coinDataTop50];

    if (sortBy.value === "change") {
        return coins.sort(
            (a, b) => parseFloat(b.changePercent24Hr) - parseFloat(a.changePercent24Hr)
        );
    }

    if (sortBy.value === "volume") {
        return coins.sort(
            (a, b) => parseFloat(b.volumeUsd24Hr) - parseFloat(a.volumeUsd24Hr)
        );
    }

    return coins;
});

</script>
<template>
    <div class="bg-[#1B2028] w-full rounded-[10px] p-[20px] mt-[20px] mb-[15px] mx-auto max-w-7xl">
        <h1 class="text-white font-bold text-3xl">Live Market</h1>

                <div class="flex gap-3 mt-4">
                    <button
                        class="bg-[#121418] text-white px-3 py-2 rounded"
                        @click="sortBy = 'change'"
                    >
                        Sort by Change %
                    </button>
                    <button
                        class="bg-[#121418] text-white px-3 py-2 rounded"
                        @click="sortBy = 'volume'"
                    >
                        Sort by Volume 24h
                    </button>
                </div>

        <div class="grid grid-cols-5 gap-4 mt-[20px] text-gray-400">
            <p>Coin</p>
            <p>Change</p>
            <p>Market Cap</p>
            <p>24h Volume</p>
            <p>Price</p>
        </div>
        <div class="max-h-[400px] overflow-y-scroll">
                    <div v-for="coin in displayedCoins" class="grid grid-cols-5 gap-4 mt-[20px] text-gray-300">
            <p>{{ coin.name }}</p>
            <p  :class="(coin.changePercent24Hr) < 0 ? 'font-bold text-red-500' : 'font-bold text-[#1ECB4F]'">{{ Number(coin.changePercent24Hr).toFixed(2)}}%</p>
            <p>${{ Number(coin.marketCapUsd).toFixed(0) }}M</p>
            <p>${{ Number(coin.volumeUsd24Hr).toFixed(0)}}M</p>
            <p>${{ Number(coin.priceUsd).toFixed(4) }}</p>
        </div>
        </div>

        <div>

        </div>
    </div>
</template>