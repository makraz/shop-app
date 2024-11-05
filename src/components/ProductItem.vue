<template>
  <div class="product">
    <img :src="'https://shop-api.ddev.site' + product.image" :alt="product.name"/>
    <h3>{{ product.name }}</h3>
    <p>{{ product.description }}</p>
    <p>{{ formattedPrice }}</p>

    <button @click="addToCart(product)">Ajouter au panier</button>
  </div>
</template>

<script>
export default {
  props: {
    product: Object
  },
  methods: {
    addToCart(product) {
      this.$emit('add-to-cart', product);
    }
  },
  computed: {
    formattedPrice() {
      return `$${this.product.price.toFixed(2)}`;
    }
  }
}
</script>

<style scoped>
.product {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}

.product img {
  max-width: 100px;
  height: auto;
}

.product button {
  margin-top: 10px;
}
</style>
