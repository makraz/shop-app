<template>
  <div class="cart">
    <h3>Votre Panier</h3>
    <div v-if="cartItems.length">
      <ul>
        <li v-for="item in cartItems" :key="item.product.id">
          {{ item.product.name }} - <input
            type="number"
            v-model.number="item.quantity"
            min="1"
            @change="updateQuantity(item, $event.target.value)"
        /> x {{ formattedPrice(item) }}
          <button @click="removeFromCart(item)">Supprimer</button>
        </li>
      </ul>
      <p>Total : {{ totalPrice }}</p>
    </div>
    <p v-else>Le panier est vide.</p>
  </div>
</template>

<script>
export default {
  props: {
    cartItems: Array
  },
  computed: {
    totalPrice() {
      console.log('totalPrice');
      return this.cartItems.reduce((total, item) => total + (item.product.price * item.quantity), 0);
    }
  },
  methods: {
    removeFromCart(item) {
      this.$emit('remove-from-cart', item);
    },
    updateQuantity(selectedItem, quantity) {
      const cartList = this.cartItems;
      const existingProduct = cartList.find(item => item.product.id === selectedItem.product.id);
      quantity = parseInt(quantity);

      if (existingProduct) {
        existingProduct.quantity = quantity;
      } else {
        cartList.push({ product: selectedItem.product, quantity: quantity });
      }
      this.$emit('update-cart', cartList);
    },
    formattedPrice(item) {
      return `$${item.product.price.toFixed(2)}`;
    }
  }
}
</script>

<style scoped>
.cart {
  padding: 20px;
  border: 1px solid #ccc;
}

.cart ul {
  list-style: none;
  padding: 0;
}

.cart li {
  margin-bottom: 10px;
}
</style>
