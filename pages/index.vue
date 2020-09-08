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
            <span>{{product.name}}</span>
            <span>{{product.price.amount}}</span>
            <span>{{product.price.measureUnit}} / {{product.price.currency}}</span>
          </v-card-text>

          <v-card-actions>
            <v-btn color="orange" text>Dodaj</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
import products from "../static/products.json";
export default {
  data() {
    return {
      products: products,
    };
  },

  methods: {
    test() {},
  },
};
</script>
