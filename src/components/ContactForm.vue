<template>
  <div class="contact-form">
    <h3>Contactez-nous</h3>
    <form @submit.prevent="submitForm">
      <div>
        <label for="email">Email</label>
        <input type="email" v-model="email" required/>
      </div>
      <div>
        <label for="message">Message</label>
        <textarea v-model="message" required maxlength="300"></textarea>
      </div>
      <button type="submit" :disabled="!email || !message">Envoyer</button>
    </form>
    <p v-if="submitted" class="success-message">Demande de contact envoyée avec succès</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      message: '',
      submitted: false
    };
  },
  methods: {
    submitForm() {
      this.submitted = true;
      // Simuler l'envoi du message
      setTimeout(() => {
        this.email = '';
        this.message = '';
      }, 1000);
    }
  }
}
</script>

<style scoped>
.contact-form {
  padding: 20px;
  border: 1px solid #ccc;
}

.contact-form form {
  display: flex;
  flex-direction: column;
}

.contact-form button {
  margin-top: 10px;
}

.success-message {
  color: green;
  margin-top: 10px;
}
</style>
