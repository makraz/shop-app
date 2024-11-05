<template>
  <div v-if="loading">Chargement...</div>
  <div class="product-list" v-else>
    <product v-for="product in products" :key="product.id" :product="product" @add-to-cart="addToCart" />
  </div>
  <div v-if="error" class="error">Erreur lors du chargement des produits : {{ error }}</div>

  <div class="pagination">
    <button @click="goToPage(currentPage - 1)" :disabled="currentPage === 1">Précédent</button>
    <button v-for="page in pageNumbers" :key="page" @click="goToPage(page)" :class="{ active: page === currentPage }">
      {{ page }}
    </button>
    <button @click="goToPage(currentPage + 1)" :disabled="currentPage === totalPages">Suivant</button>
  </div>
</template>

<script>
import Product from './ProductItem.vue';
import axios from 'axios';

export default {
  components: {
    Product
  },
  mounted() {
    this.fetchProducts();
  },
  data() {
    return {
      products: [],
      loading: false,
      error: null,
      cart: [],
      currentPage: 1,
      pageSize: 10,
      totalItems: 0,
      totalPages: 0,
    };
  },
  methods: {
    async fetchProducts() {
      this.loading = true;
      this.error = null;
      try {
        const response = await axios.get('https://shop-api.ddev.site/api/products', {
          params: {
            page: this.currentPage,
            limit: this.pageSize,
          },
        });
        this.products = response.data;
        this.totalItems = parseInt(response.headers['x-total-count']);
        this.totalPages = Math.ceil(this.totalItems / this.pageSize);
      } catch (err) {
        this.error = err.message || 'Une erreur est survenue';
      } finally {
        this.loading = false;
      }
    },
    goToPage(page) {
      if (page < 1 || page > this.totalPages) return; // Vérifie si la page est valide
      this.currentPage = page;
      this.fetchProducts(); // Rechargement des produits pour la nouvelle page
    },
    addToCart(product) {
      const existingProduct = this.cart.find(item => item.product.id === product.id);
      if (existingProduct) {
        existingProduct.quantity++;
      } else {
        this.cart.push({ product: product, quantity: 1 });
      }
      this.$emit('update-cart', this.cart);
    }
  },
  computed: {
    pageNumbers() {
      let pages = [];
      for (let i = 1; i <= this.totalPages; i++) {
        pages.push(i);
      }
      return pages;
    }
  }
}
</script>

<style scoped>
.product-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
</style>
