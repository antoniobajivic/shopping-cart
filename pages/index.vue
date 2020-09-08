<template>
  <v-container fluid>
    <v-row>
      <v-spacer></v-spacer>
      <v-btn color="blue" class="white--text" @click="toggleCart">
        Košarica
        <v-icon right dark>mdi-cart</v-icon>
      </v-btn>
    </v-row>
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
            <v-btn
              class="white--text"
              color="green"
              shaped
              depressed
              @click="addToCart(product)"
            >Dodaj</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-navigation-drawer v-model="cartDrawer" right app>
      <span class="font-weight-bold font-italic" style="font-size: 24px;">Košarica</span>
      <div v-for="(cartItem,index) in cart" :key="index">
        <v-card class="mx-auto" max-width="344" outlined>
          <v-list-item three-line>
            <v-list-item-content>
              <v-list-item-title class="headline mb-1">{{cartItem.name}}</v-list-item-title>
              <v-list-item-subtitle>
                <span>{{cartItem.quantity}}x</span>
                <span>{{cartItem.price.amount}}{{cartItem.price.currency}}</span></v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-avatar tile size="80" color="grey"><v-img :src="cartItem.image" /></v-list-item-avatar>
          </v-list-item>

          <v-card-actions>
            <v-btn text @click="removeFromCart(index)" color="red" outlined rounded>Ukloni</v-btn>
          </v-card-actions>
        </v-card>
      </div>
      <v-divider></v-divider>
      <span >Ukupno: {{finalPrice}} Kn</span>
    </v-navigation-drawer>
  </v-container>
</template>


<script>
import products from "../static/products.json";
import cloneDeep from "lodash/cloneDeep";
export default {
  data() {
    return {
      products: products,
      cart: [],
      cartDrawer: false,
      finalPrice: 0
    };
  },

  methods: {
    addToCart(product) {
      let hasItem = false;
      this.cart.forEach((item) => {
        if (item.id === product.id) {
          hasItem = true;
        }
      });
      if (hasItem) {
        let index = this.cart
          .map(function (e) {
            return e.id;
          })
          .indexOf(product.id);
        this.cart[index].quantity += 1;
        this.cart[index].price.amount += product.price.amount;
        this.cart[index].price.amount = Math.round(this.cart[index].price.amount * 1e2) /1e2;
      } else {
        product.quantity = 1;
        let productClone = cloneDeep(product);
        this.cart.push(productClone);
      }
    },
    removeFromCart(index){
      this.cart.splice(index, 1)
    },
    toggleCart(){
      this.cartDrawer = !this.cartDrawer
    }
  },
  watch: {
    cart: {
      deep: true,
      handler() {
        this.finalPrice = 0;
        this.cart.forEach((item) => {
          this.finalPrice += item.price.amount;
        });
        this.finalPrice = this.finalPrice.toFixed(2);
      },
    },
  },
};
</script>
