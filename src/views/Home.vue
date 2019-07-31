<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>ImageUrl: <input type="text" v-model="newProductImageUrl"></p>
    <button v-on:click="makeProduct()">Make a new product</button>
    <div v-for="product in products">
      <p>name:{{product.name}}</p>
      <p>image_url: {{product.image_url}}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <img width="150px" v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
      <p><button v-on:click="setRecipe(recipe)">Show more info</button></p>
      <div v-if="currentRecipe === recipe">
        <p>directions: {{ recipe.directions }}</p>
        <p>ingredients {{ recipe.ingredients }}</p>
        <p>prep time {{ recipe.prep_time }}</p>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios"
export default {
  data: function() {
    return {
      message: "change.js!",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      currentProduct: {}
    };
  },
  created: function() {
    console.log('i am in created');
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    })
  },
  methods: {
    makeProduct: function() {
      console.log('in make product');
      var newProduct = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl
      }
      
      axios.post('/api/products', newProduct).then(response => {
        console.log('in the callback for create')
        console.log(response.data);
        this.recipes.push(response.data);
      })
    },
    setProduct: function(theProduct) {
      console.log('setting the product');
      console.log(theProduct);
      this.currentProduct = theProduct;
    }
  }
};
</script>