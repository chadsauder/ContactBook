<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="./assets/book.png"
          transition="scale-transition"
          width="40"
        />

        <v-text>
          Contact Book
        </v-text>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <ContactBook/>
    </v-main>
  </v-app>
</template>

<script>
import ContactBook from './components/contactBook.vue';

export default {
  name: 'App',

  components: {
    ContactBook
},

  data: () => ({
    //
  }),
};
</script>
