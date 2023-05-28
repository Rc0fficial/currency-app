<template>
  <div class="container mx-auto mt-6">
    <h1 class="font-bold text-3xl">Search Old Currency</h1>
    <div class="flex items-center justify-between mt-6 mb-12 gap-12">
      <div class="flex gap-6">
        <div class="relative">
          <label for="countryFilter" class="block mb-2 font-semibold"
            >Country</label
          >
          <select
            id="countryFilter"
            v-model="selectedCountry"
            class="block appearance-none w-full px-4 py-2 border border-gray-300 rounded-md"
          >
            <option value="">All Country</option>
            <option
              v-for="country in countries"
              :value="country"
              class="border-t border-gray-300"
            >
              {{ country }}
            </option>
            <option value="">All Countries</option>
          </select>
          <div
            class="absolute inset-y-0 mt-7 ml-4 right-0 flex items-center px-2 pointer-events-none"
          >
            <ChevronDownIcon class="w-4 h-4 text-gray-500" />
          </div>
        </div>
        <div class="relative">
          <label for="yearFilter" class="block mb-2 font-semibold">Year</label>
          <div class="custom-dropdown">
            <div class="selected-year" @click="toggleDropdown">
              {{ selectedYear || "All years" }}
            </div>
            <div v-show="isDropdownOpen" class="dropdown-options">
              <div
                v-for="year in years"
                :key="year"
                :class="['year-option', { selected: year === selectedYear }]"
                @click="selectYear(year)"
              >
                {{ year }}
              </div>
            </div>
          </div>
          <div
            class="absolute inset-y-0 mt-7 ml-4 right-0 flex items-center px-2 pointer-events-none"
          >
            <ChevronDownIcon class="w-4 h-4 text-gray-500" />
          </div>
        </div>
        <div class="relative">
          <label for="statusFilter" class="block mb-2 font-semibold"
            >Status</label
          >
          <select
            id="statusFilter"
            v-model="selectedStatus"
            class="block appearance-none w-full px-4 py-2 border border-gray-300 rounded-md"
          >
            <option value="">All statuses</option>
            <option
              v-for="status in statuses"
              :value="status"
              class="border-t border-gray-300"
            >
              {{ status }}
            </option>
          </select>
          <div
            class="absolute inset-y-0 mt-7 ml-4 right-0 flex items-center px-2 pointer-events-none"
          >
            <ChevronDownIcon class="w-4 h-4 text-gray-500" />
          </div>
        </div>
        <div class="relative">
          <label for="valueFilter" class="block mb-2 font-semibold"
            >Notes Range</label
          >
          <input
            id="valueFilter"
            type="range"
            v-model="selectedValueRange"
            min="0"
            max="100"
            class="block w-full"
          />
          <div class="mt-2 flex justify-between">
            <span>{{ selectedValueRange[0] }}</span>
            <span>{{ selectedValueRange[1] }}</span>
          </div>
        </div>
      </div>
      <div class="mb-4">
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          @click="clearFilters"
        >
          Clear All
        </button>
      </div>
    </div>
    <div
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4"
    >
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
import ChevronDownIcon from "@/components/icons/ChevronDownIcon.vue";
import Datepicker from "vue3-datepicker";
import "vue3-datepicker/dist/vue3-datepicker.css";

export default defineComponent({
  components: {
    CurrencyCard,
    ChevronDownIcon,
    Datepicker,
  },
  data() {
    return {
      currencies: currencyData,
      selectedCountry: "",
      selectedYear: "",
      isDropdownOpen: false,
      selectedStatus: "",
      selectedValueRange: [0, 100],
    };
  },
  computed: {
    countries() {
      return [
        ...new Set(this.currencies.map((currency) => currency.country_name)),
      ];
    },
    years() {
      const startYear = 1900;
      const endYear = 2023;
      const years = [];
      for (let year = startYear; year <= endYear; year++) {
        years.push(year.toString());
      }
      return years;
    },

    statuses() {
      return [...new Set(this.currencies.map((currency) => currency.status))];
    },
    filteredCurrencies() {
      return this.currencies.filter((currency) => {
        return (
          (!this.selectedCountry ||
            currency.country_name === this.selectedCountry) &&
          (!this.selectedYear || currency.year === this.selectedYear) &&
          (!this.selectedStatus || currency.status === this.selectedStatus) &&
          currency.value >= this.selectedValueRange[0] &&
          currency.value <= this.selectedValueRange[1]
        );
      });
    },
  },
  methods: {
    clearFilters() {
      this.selectedCountry = "";
      this.selectedYear = "";
      this.selectedStatus = "";
      this.selectedValueRange = [0, 100];
    },
    selectYear(year) {
      this.selectedYear = year;
      this.isDropdownOpen = false;
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
  },
});
</script>
<style>
.custom-dropdown {
  position: relative;
  display: inline-block;
}

.selected-year {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
}

.dropdown-options {
  position: absolute;
  top: calc(100% + 8px); /* Position the dropdown below the input */
  left: 50%; /* Center the dropdown relative to the input */
  transform: translateX(-50%); /* Adjust the horizontal centering */
  z-index: 10;
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Display options in three columns */
  gap: 8px; /* Add gap between options */
  max-height: 248px;
  overflow-y: auto;
  border: 1px solid #ccc;
  padding: 8px;
  
  border-radius: 4px;
  background-color: #fff;
  margin-top: 8px;

  /* Scrollbar Styling */
  scrollbar-width: thin; /* Set the width of the scrollbar */
  scrollbar-color: #ccc transparent; /* Set the color of the scrollbar track and thumb */
}

.dropdown-options::-webkit-scrollbar {
  width: 8px; /* Set the width of the scrollbar */
}

.dropdown-options::-webkit-scrollbar-track {
  background-color: transparent; /* Set the background color of the scrollbar track */
}

.dropdown-options::-webkit-scrollbar-thumb {
  background-color: #ccc; /* Set the color of the scrollbar thumb */
}

.year-option {
  padding: 8px 12px;
  cursor: pointer;
}

.year-option:hover {
  background-color: blue;
  color: white;
  border-radius: 4px; /* Add rounded border */
}

.year-option.selected {
  background-color: blue;
  color: white;
}


</style>
