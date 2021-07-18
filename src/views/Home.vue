<template>
  <div class="home">
    <h1>Simple Products App</h1>
    <hr>
    <div v-for="product in products" v-bind:key="product.id">
      <p>Product name: {{ product.name }}</p>
      <p>Product price: {{ product.price }}</p>
      <hr>
    </div>
  </div>
</template>
<style></style>
<script>
export default {
  data: function () {
    return {
      products: [
        { id: 1, name: "Product 1", price: 10 },
        { id: 2, name: "Product 2", price: 230 },
      ],
    };
  },
  created: function () {},
  methods: {},
};
</script>
