<template>
  <div class="home">
    <h1>Simple Products App</h1>
    <p>Name: <input type="text" v-model="newProduct.name" /></p>
    <p>Description: <input type="text" v-model="newProduct.description" /></p>
    <p>Price: <input type="text" v-model="newProduct.price" /></p>
    <p>image_url: <input type="text" v-model="newProduct.image_url" /></p>
    <button v-on:click="productsCreate">Add Product</button>
    <hr />
    <div v-for="product in products" :key="product.id">
      <p>Product: {{ product.name }}</p>
      <img v-bind:src="product.image_url" />
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <hr />
    </div>
  </div>
</template>
<style>
img {
  height: 250px;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
      newProduct: {},
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
    productsCreate: function () {
      axios
        .post("http://localhost:3000/products", this.newProduct)
        .then((response) => {
          this.products.push(response.data);
          this.newProduct = {};
        });
    },
  },
};
</script>
