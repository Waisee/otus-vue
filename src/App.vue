<template>
  <div class="container d-flex justify-content-center mt-4">
    <div v-if="loading" class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div v-if="!loading" class="row row-cols-4 g-4">
      <Product 
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>

</template>

<script setup>
import { ref } from 'vue';
import Product from './components/Product.vue'

const products = ref({});
const loading = ref(true);

fetch('https://fakestoreapi.com/products')
  .then(response => {
    if (response.status !== 200) {
      console.log(response.status);

      return;
    }
    response.json().then(json => {
      products.value = json;
      loading.value = false;
    })
  })
  .catch(err => {
    console.log(err);
  });
</script>
