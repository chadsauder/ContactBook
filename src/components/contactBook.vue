<template>
  <div>
    <v-text class="ml-2 text-h5" style="font-weight: bold"> Add a new contact</v-text>
    <v-spacer></v-spacer>
    <div class="mb-4">
      <v-form ref="form" v-model="valid" lazy-validation style="width: 50%" class="ml-3">
        <v-text-field
          v-model="name"
          :counter="30"
          :rules="nameRules"
          label="Name"
          required
        ></v-text-field>

        <v-text-field v-model="address" label="Address" required></v-text-field>

        <v-text-field
          v-model="phone"
          :counter="12"
          :rules="phoneRules"
          label="Phone"
          required
        ></v-text-field>

        <v-text-field
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
        ></v-text-field>

        <v-select
          v-model="category"
          :items="categories"
          :rules="[(v) => !!v || 'Item is required']"
          label="Category"
          required
        ></v-select>

        <v-btn :disabled="!valid" color="success" class="mr-4" @click="addNewContact()">
          Add
        </v-btn>

        <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
      </v-form>
    </div>
    <hr />
    <div class="mt-4">
      <v-text class="ml-2 text-h5 mb-3" style="font-weight: bold">My Contacts</v-text>
      <div
        v-for="(contact, index) in contacts"
        :key="index"
        class="ml-3"
        style="width: 50%"
      >
        <v-card outlined shaped elevation="2" class="mb-3 mt-3">
          <v-card-title>{{ contact.name }}</v-card-title>
          <v-card-subtitle> Phone number: {{ contact.phone }} </v-card-subtitle>
          <v-card-subtitle> Email: {{ contact.email }} </v-card-subtitle>
          <v-card-subtitle> Address: {{ contact.address }} </v-card-subtitle>
          <v-card-subtitle> Type: {{ contact.category }} </v-card-subtitle>
          <v-btn
            color="success"
            class="mr-4 ml-3 mb-3"
            @click="editContact(index, contact)"
          >
            Edit
          </v-btn>
          <v-btn color="error" class="mr-4 ml-3 mb-3" @click="deleteContact(index)">
            delete
          </v-btn>
        </v-card>
      </div>
    </div>

    <v-dialog v-model="editdialog" v-if="currentContact != null" persistent>
      <v-card>
        <v-form ref="form" style="width: 50%" class="ml-3">
          <v-text-field v-model="currentContact.name" required></v-text-field>

          <v-text-field v-model="currentContact.address" required></v-text-field>

          <v-text-field v-model="currentContact.phone" required></v-text-field>

          <v-text-field v-model="currentContact.email" required></v-text-field>

          <v-select
            v-model="currentContact.category"
            :items="categories"
            required
          ></v-select>

          <v-btn :disabled="!valid" color="success" class="mr-4" @click="updateContact()">
            update
          </v-btn>
        </v-form>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    editdialog: false,
    name: "",
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 30) || "Name must be less than 30 characters",
    ],
    phone: "",
    phoneRules: [(v) => !!v || "Phone number is required", (v) => v && v.length <= 12],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    address: "",

    category: "",
    categories: ["Family", "Friend", "Business", "Co-worker", "Other"],
    contacts: [],
    currentContact: null,
  }),
  mounted() {
    if (localStorage.contacts) {
      this.contacts = JSON.parse(localStorage.contacts);
    }
  },
  watch: {
    contacts: {
      handler(newContacts) {
        localStorage.contacts = JSON.stringify(newContacts);
      },
      deep: true,
    },
  },
  methods: {
    addNewContact() {
      this.contacts.push({
        name: this.name,
        phone: this.phone,
        email: this.email,
        address: this.address,
        category: this.category,
      });
      console.log("These are my contacts", this.contacts);
    },
    deleteContact(index) {
      this.$delete(this.contacts, index);
    },
    editContact(index, contact) {
      console.log(index);
      console.log(contact);
      this.editdialog = true;
      this.currentContact = contact;
    },
    updateContact() {
      this.editdialog = false;
      this.currentContact = null;
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
