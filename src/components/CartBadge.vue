<template>
  <div class="cart-badge" @click="toggleCart">
    Panier ({{ cartItemCount }})
  </div>
</template>

<script>
export default {
  props: {
    cartItems: Array
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => total + item.quantity, 0);
    }
  },
  methods: {
    toggleCart() {
      this.$emit('toggle-cart');
    }
  }
}
</script>

<style scoped>
.cart-badge {
  position: absolute;
  top: 20px;
  right: 20px;
  background-color: red;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
}
</style>
