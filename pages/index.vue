<script setup></script>
<script setup>
import { ref } from 'vue'
const products = ref([
  {
    "id": "4a0c8a36-4d74-4eeb-9b38-8c9a9d2b2f68",
    "type": "A4",
    "title": "Impression feuille A4",
    "price": 9.99,
    "stripe_url": "https://example.com/stripe"
  },
  {
    "id": "c24d54f9-7383-42bb-97f3-ae8f95a0cebc",
    "type": "A3",
    "title": "Impression feuille A3",
    "price": 19.99,
    "stripe_url": "https://example.com/stripe"
  },
  {
    "id": "ae4c8a8f-b697-4b2f-bb6d-46e8e9ed9a98",
    "type": "A5",
    "title": "Impression feuille A5",
    "price": 4.99,
    "stripe_url": "https://example.com/stripe"
  }
]);
</script>


<template>
  <Header />
  <div class="container mx-auto">
    <div class="grid items-center justify-between w-full grid-cols-1 gap-3 px-2 mx-auto my-8 md:my-24 lg:grid-cols-3"
      style="max-width:1000px;">
      <div v-for="item in products" :key="item.id"
        class="w-full px-8 pt-6 pb-4 text-base text-center transition border rounded-lg shadow-md cursor-pointer hover:border-sky-400 hover:shadow-lg"
        style="min-width: 300px;">
        <img src="paper.png" alt="paper" class="h-24 mx-auto mb-4">
        <p class="mb-1 font-medium">{{ item.title }}</p>
        <p class="mb-1 text-slate-500">Format: {{ item.type }}</p>
        <p class="mb-4 font-semibold">{{ item.price }} €</p>
        <button class="mb-2">Commander</button>
        <p class="mb-2 text-slate-500" style="font-size: 12px;">Pour Lundi 20 Mars, 11h00</p>
      </div>
    </div>
    <Footer />
  </div>
</template>

<style lang='scss'></style>