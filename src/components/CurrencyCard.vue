<template>
  <div
    class="currency-card border-2 border-solid rounded-lg overflow-hidden shadow-xl border-slate-200"
  >
    <img :src="currency.front_pic" alt="Front" class="currency-card-image" />
    <div class="mx-3 mt-4 mb-6">
      <h3 class="font-bold text-slate-600">{{ currency.country_name }}</h3>
      <p class="text-slate-600 text-sm font-semibold">{{ currency.year }}</p>
      <p class="text-slate-600 text-sm font-semibold">{{ currency.status }}</p>
      <p class="text-sm text-slate-600">{{ currency.history }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currency: Object,
  },
};
</script>
