<template>
  <div class="container justify-content-center mt-4">
    <div v-if="error">
      <p>Something went wrong</p>
    </div>
    <div v-else>
      <div v-if="loading" class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
      <div v-if="!loading">
        <button @click="order = !order" class="btn btn-success mb-4">{{!order ? 'Cart (' + cart.length + ')' : 'Catalog'}}</button>
        <div v-if="!order">
          <div class="row g-4 mb-4">
            <Header @search="search = $event" />
          </div>
          <AddForm @submit="productsFiltered.push(Object($event))" />
          <div class="row row-cols-4 g-4">
            <Product @addToCart="cart.push($event)" v-for="product in productsFiltered" :key="product.id" :product="product" />
          </div>
        </div>
        <div v-if="order">
          <Cart @order="ordered" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onBeforeMount } from 'vue';
import Product from './components/Product.vue'
import Header from './components/Header.vue'
import AddForm from "./components/AddForm.vue";
import Cart from "./components/Cart.vue";

const products = ref([]);
const productsFiltered = ref([]);
const error = ref(false);
const loading = ref(true);
const search = ref('');
const cart = ref([]);
const order = ref(false);

watch(search, (newSearch) => {
  productsFiltered.value = products.value.filter((product) => {
    if (product.title.includes(newSearch) || product.price === newSearch) {
      return true;
    }
  });
});

onBeforeMount(async () => {
  await fetch('https://fakestoreapi.com/products')
    .then(response => {
      if (response.status !== 200) {
        error.value = true;
        return;
      }
      response.json().then(json => {
        products.value = json;
        productsFiltered.value = products.value;
        loading.value = false;
      })
    })
    .catch(err => {
      error.value = true;
    });
})

function ordered() {
  cart.value = [];
  order.value = false;
}
</script>
