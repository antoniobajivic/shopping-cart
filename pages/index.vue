<template>
<div>{{products}}</div>
</template>


<script>
import products from "../static/products.json"
export default {
  	data(){
      return{
        products: products
      }
    }
}
</script>
