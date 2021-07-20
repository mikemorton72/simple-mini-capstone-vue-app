<template>
  <div class="home">
    <h1>Simple Products App</h1>
    <h2>Create a new Product</h2>
    <p>Name: <input type="text" v-model="newProduct.name" /></p>
    <p>Description: <input type="text" v-model="newProduct.description" /></p>
    <p>Price: <input type="text" v-model="newProduct.price" /></p>
    <p>image_url: <input type="text" v-model="newProduct.image_url" /></p>
    <button v-on:click="createProduct">Add Product</button>
    <hr />
    <div v-for="product in sortedProducts" v-bind:key="product.id">
      <p>Product: {{ product.id }}</p>
      <p>{{ product.name }}</p>
      <img v-bind:src="product.image_url" />
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <p><button v-on:click="showProduct(product)">Show Info</button></p>
      <p><button v-on:click="editProduct(product)">Update</button></p>
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
    <dialog id="product-update">
      <form method="dialog">
        <h2>{{ currentProduct.name }}</h2>
        <p>Name: <input type="text" v-model="currentProduct.name" /></p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>Price: <input type="text" v-model="currentProduct.price" /></p>
        <p>
          Image URL: <input type="text" v-model="currentProduct.image_url" />
        </p>
        <p>
          <button v-on:click="updateProduct(currentProduct)">Update</button>
        </p>
        <p>
          <button v-on:click="destroyProduct(currentProduct)">Delete</button>
        </p>
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
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
    createProduct: function () {
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
    showProduct: function (product) {
      this.currentProduct = product;
      console.log(this.currentProduct);
      document.querySelector("#product-details").showModal();
    },
    editProduct: function (product) {
      this.currentProduct = product;
      document.querySelector("#product-update").showModal();
    },
    updateProduct: function (product) {
      axios
        .patch(
          `http://localhost:3000/products/${product.id}`,
          this.currentProduct
        )
        .then((response) => {
          console.log(response.data);
        });
    },
    destroyProduct: function (product) {
      axios
        .delete(`http://localhost:3000/products/${product.id}`)
        .then((response) => {
          var index = this.products.indexOf(product);
          this.products.splice(index, 1);
          console.log(response.data);
        });
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
