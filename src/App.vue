<template>
  <div class="container">
    <div class="row">
      <h1 class="p-0 mt-4">API CRYPTO</h1>

      <input type="text" class="form-control bg-dark text-light rounded-0 border-0 mt-4" placeholder="Search Coin..." @keyup="searchCoin()" v-model="textSearch">

      <table class="table table-dark mt-4">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title.toUpperCase() }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">{{ index + 1}}</td>
            <td>
              <img :src="coin.image" style="width: 2rem;" class="me-2">
              <span>{{ coin.name }}</span>
              <span class="ms-2 text-uppercase text-muted">{{ coin.symbol }}</span>
            </td>
            <td>$ {{ coin.current_price }}</td>
            <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">{{ coin.price_change_percentage_24h }} %</td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&locale=en')
    const data = await res.json()
    this.coins = data
    this,this.filteredCoins = data
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(coin =>
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style>

</style>
