<template>
  <div
    class="currency-card border-2 border-solid rounded-lg overflow-hidden shadow-xl border-slate-200"
  >
    <img
      :src="currency.front_pic"
      alt="Front"
      class="currency-card-image h-[150px] w-full"
    />
    <div class="mx-3 mt-4 mb-6">
      <div class="flex justify-between">
        <div class="flex flex-col">
          <h3 class="font-bold text-slate-600">{{ currency.country_name }}</h3>
          <p class="text-slate-600 text-sm font-normal">
            {{ currency.year }}
          </p>
        </div>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white py-1 px-2 rounded"
        >
          Bid
        </button>
      </div>
      <p class="text-slate-600 text-sm my-2 font-normal">
        {{ currency.status }}
      </p>
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
