<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContactParams: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("http://localhost:3000/api/contacts.json").then((response) => {
        this.contacts = response.data;
        console.log("Contacts Index", this.contacts);
      });
    },
    createContact: function () {
      axios.post("http://localhost:3000/api/contacts.json", this.newContactParams).then((response) => {
        this.contacts.push(response.data);
        console.log("Contact created.", response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div v-for="contact in contacts" :key="contact">
      <p>{{ contact }}</p>
    </div>
    <div>
      First Name:
      <input type="text" v-model="newContactParams.first_name" />
    </div>
    <div>
      Last Name:
      <input type="text" v-model="newContactParams.last_name" />
    </div>
    <div>
      Email:
      <input type="text" v-model="newContactParams.email" />
    </div>
    <div>
      Phone Number:
      <input type="text" v-model="newContactParams.phone_number" />
    </div>

    <div>
      <button v-on:click="createContact">Add Contact</button>
    </div>
  </div>
</template>

<style></style>
