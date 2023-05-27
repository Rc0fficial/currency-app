<template>
  <div class="container mx-auto mt-6">
    <h1 class="font-bold text-3xl">Search Old Currency</h1>
    <div>
      <label for="countryFilter">Country:</label>
      <select id="countryFilter" v-model="selectedCountry">
        <option value="">Select Country</option>
        <option v-for="country in countries" :value="country">
          {{ country }}
        </option>
      </select>
    </div>
    <div>
      <label for="yearFilter">Filter by Year:</label>
      <select id="yearFilter" v-model="selectedYear">
        <option value="">All</option>
        <option v-for="year in years" :value="year">{{ year }}</option>
      </select>
    </div>
    <div class="mb-4">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="clearFilters">Clear Filters</button>
    </div>
    <div class="grid grid-cols-3 gap-4">
      <CurrencyCard
        v-for="currency in filteredCurrencies"
        :key="currency.country_name"
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
      selectedYear: "",
    };
  },
  computed: {
    countries() {
      return [...new Set(this.currencies.map((currency) => currency.country_name))];
    },
    years() {
      return [...new Set(this.currencies.map((currency) => currency.year))];
    },
    filteredCurrencies() {
      return this.currencies.filter((currency) => {
        return (
          (!this.selectedCountry || currency.country_name === this.selectedCountry) &&
          (!this.selectedYear || currency.year === this.selectedYear)
        );
      });
    },
  },
  methods: {
    clearFilters() {
      this.selectedCountry = "";
      this.selectedYear = "";
    },
  },
});
</script>
