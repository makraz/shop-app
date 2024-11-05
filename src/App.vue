<template>
  <!-- Barre de navigation -->
  <nav>
    <router-link to="/">Accueil</router-link>
    <router-link to="/contact">Contact</router-link>
  </nav>

  <!-- Badge du panier -->
  <cart-badge :cart-items="cartItems" @toggle-cart="toggleCart"/>

  <!-- Contenu du panier -->
  <shopping-cart v-if="isCartVisible" :cart-items="cartItems" @remove-from-cart="removeFromCart"/>

  <!-- Vue des produits -->
  <product-list @update-cart="updateCart"/>

  <!-- Message de succès après l'envoi du formulaire -->
  <p v-if="isFormSent" class="success-message">Demande de contact envoyée avec succès</p>
</template>

<script>
import CartBadge from './components/CartBadge.vue';
import ShoppingCart from './components/ShoppingCart.vue';
import ProductList from './components/ProductList.vue';
// import ContactForm from './components/ContactForm.vue';

export default {
  name: 'App',
  components: {
    CartBadge,
    ShoppingCart,
    ProductList
    // ,
    // ContactForm
  },
  data() {
    return {
      cartItems: [],           // Initialisation de cartItems comme tableau vide
      isCartVisible: false,    // Etat de visibilité du panier
    };
  },
  methods: {
    // Méthode pour afficher ou masquer le panier
    toggleCart() {
      this.isCartVisible = !this.isCartVisible;
    },

    // Méthode pour mettre à jour le panier (ajouter ou supprimer des produits)
    updateCart(updatedCart) {
      this.cartItems = updatedCart;
    },

    // Méthode pour supprimer un produit du panier
    removeFromCart(item) {
      const index = this.cartItems.indexOf(item);
      if (index !== -1) {
        this.cartItems.splice(index, 1);
      }
    }
  }
}
</script>

<style scoped>
/* Style de la barre de navigation */
nav {
  display: flex;
  justify-content: space-around;
  padding: 10px;
  background-color: #333;
  color: white;
}

nav a {
  text-decoration: none;
  color: white;
  padding: 5px 10px;
}

nav a:hover {
  background-color: #555;
  border-radius: 4px;
}

/* Style du message de succès */
.success-message {
  color: green;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
  font-size: 1.2rem;
}

/* Centrer le contenu */
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}
</style>
