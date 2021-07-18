<template>
  <div class="home">
    <h1>Simple Products App</h1>
    <h2>Create a new Product</h2>
    <p>Name: <input type="text" v-model="newProduct.name" /></p>
    <p>Description: <input type="text" v-model="newProduct.description" /></p>
    <p>Price: <input type="text" v-model="newProduct.price" /></p>
    <p>image_url: <input type="text" v-model="newProduct.image_url" /></p>
    <button v-on:click="productsCreate">Add Product</button>
    <hr />
    <div v-for="product in sortedProducts" v-bind:key="product.id">
      <p>Product: {{ product.id }}</p>
      <p>{{ product.name }}</p>
      <img v-bind:src="product.image_url" />
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <button v-on:click="productsShow(product)">Show Info</button>
      <hr />
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h2>{{ currentProduct.name }}</h2>
        <img v-bind:src="currentProduct.image_url" />
        <p>{{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <button>Close</button>
      </form>
    </dialog>
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
      currentProduct: {},
      errors: {},
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
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    productsShow: function (product) {
      this.currentProduct = product;
      console.log(this.currentProduct);
      document.querySelector("#product-details").showModal();
    },
  },
  computed: {
    sortedProducts: function () {
      var sortProducts = this.products;
      return sortProducts.sort((a, b) => a.id - b.id);
    },
  },
};
</script>
