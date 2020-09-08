<template>
  <v-container fluid>
    <v-row>
      <v-col
        cols="12"
        sm="6"
        md="4"
        lg="3"
        v-for="(product, index) in products.products"
        :key="index"
      >
        <v-card class="mx-auto" min-height="350px">
          <v-img
            style="object-fit: contain"
            class="white--text align-end"
            height="200px"
            :src="product.image"
          ></v-img>

          <v-card-text class="text--primary">
            <div style="text-align: center">
              <span class="font-weight-bold">{{product.name}}</span>
              <br />
              <span>{{product.price.amount}}</span>
              <span>{{product.price.measureUnit}} / {{product.price.currency}}</span>
            </div>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn class="white--text" color="green" shaped depressed @click="addToCart(product)">Dodaj</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-col>
      <div v-for="item in cart" :key="item.id">
        <span>{{item}}</span>
      </div>
    </v-row>
  </v-container>
</template>


<script>
import products from "../static/products.json";
import cloneDeep from "lodash/cloneDeep"
export default {
  data() {
    return {
      products: products,
      cart: []
    };
  },

  methods: {
    addToCart(product) {
      let hasItem = false;
      this.cart.forEach(item => {
        if(item.id === product.id){
          hasItem = true
        }
      });
      if(hasItem){
        let index = this.cart.map(function(e) { return e.id; }).indexOf(product.id);
        this.cart[index].quantity += 1
        this.cart[index].price.amount += product.price.amount
      }
      else{
        product.quantity = 1
        let productClone = cloneDeep(product)
        this.cart.push(productClone)
      }
    },
  },
};
</script>
