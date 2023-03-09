<script lang="ts">
    async function getCoins() {
        const res = await fetch(
            "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=16&page=1&sparkline=false"
        );
        const coins = await res.json();

        if (res.ok) {
            return coins;
        } else {
            throw new Error(coins);
        }
    }

    $: allCoinsPromise = getCoins();
</script>

<section class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 p-4">
    {#await allCoinsPromise then coins}
        {#each coins as coin}
            <div
                class="bg-white rounded-lg overflow-hidden shadow-md cursor-pointer hover:shadow-lg transform hover:-translate-y-1 hover:scale-110 duration-300"
            >
                <div class="flex justify-center pt-4">
                    <img
                        class="w-20"
                        src={coin.image}
                        alt={coin.name + " logo"}
                    />
                </div>
                <div class="p-4">
                    <p class="text-xl font-bold mb-2">{coin.name}</p>
                    <p class="text-gray-700 text-base mb-2">
                        {coin.symbol.toUpperCase()}
                    </p>
                    <p class="text-green-600 text-2xl font-bold">
                        ${coin.current_price.toFixed(2)}
                        <span
                            class="bg-gradient-to-r from-blue-500 to-green-400 text-transparent bg-clip-text text-sm"
                            >USD</span
                        >
                    </p>
                </div>
            </div>
        {/each}
    {/await}
</section>
