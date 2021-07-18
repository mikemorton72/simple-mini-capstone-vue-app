<template>
  <div class="home">
    <h1>Simple Products App</h1>
    <hr />
    <div v-for="product in products" v-bind:key="product.id">
      <p>Product name: {{ product.name }}</p>
      <p>Product price: {{ product.price }}</p>
      <hr />
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
    this.productsIndex();
  },
  methods: {
    productsIndex: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
  },
};
</script>
