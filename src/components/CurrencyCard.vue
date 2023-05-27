<template>
    <div class="currency-card">
      <div class="currency-card-images">
        <img :src="currency.front_pic" alt="Front" class="currency-card-image" />
        <img :src="currency.back_pic" alt="Back" class="currency-card-image" />
      </div>
      <h3>{{ currency.country_name }}</h3>
      <p>Year: {{ currency.year }}</p>
      <p>Status: {{ currency.status }}</p>
      <p>Value: {{ currency.value }}</p>
      <p>{{ currency.history }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      currency: Object,
    },
  };
  </script>
  
  <style scoped>
  .currency-card {
    @apply border-2 border-gray-300 p-4 mb-4;
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 10px;
  }
  
  .currency-card-images {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 10px;
  }
  
  .currency-card-image {
    max-width: 100%;
  }
  </style>
  