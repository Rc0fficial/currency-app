<template>
  <div>
    <div>
      <label for="countryFilter">Filter by Country:</label>
      <select id="countryFilter" v-model="selectedCountry">
        <option value="">All</option>
        <option v-for="country in countries" :value="country">
          {{ country }}
        </option>
      </select>
    </div>
    <div>
      <label for="priceFilter">Filter by Price:</label>
      <select id="priceFilter" v-model="selectedPrice">
        <option value="">All</option>
        <option v-for="price in prices" :value="price">{{ price }}</option>
      </select>
    </div>
    <div>
      <CurrencyCard
        v-for="currency in filteredCurrencies"
        :key="currency.name"
        :currency="currency"
      />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import CurrencyCard from "./components/CurrencyCard.vue";
import currencyData from "./currencies.json";

export default defineComponent({
  components: {
    CurrencyCard,
  },
  data() {
    return {
      currencies: currencyData,
      selectedCountry: "",
      selectedPrice: "",
    };
  },
  computed: {
    countries() {
      return [...new Set(this.currencies.map((currency) => currency.country))];
    },
    prices() {
      return [...new Set(this.currencies.map((currency) => currency.price))];
    },
    filteredCurrencies() {
      return this.currencies.filter((currency) => {
        return (
          (!this.selectedCountry ||
            currency.country === this.selectedCountry) &&
          (!this.selectedPrice ||
            currency.price === parseInt(this.selectedPrice))
        );
      });
    },
  },
});
</script>
