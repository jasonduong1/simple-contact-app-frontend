<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Contacts",
      contacts: [],
      newContactParams: {},
      currentContact: {},
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
    showContact: function (contact) {
      console.log(contact);
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal();
    },
    showContactUpdate: function (contact) {
      console.log(contact);
      this.currentContact = contact;
      document.querySelector("#contact-update").showModal();
    },
    updateContact: function (contact) {
      let editContactParams = contact;
      axios.patch("http://localhost:3000/api/contacts/" + contact.id + ".json", editContactParams).then((response) => {
        console.log("Updated!", response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div v-for="contact in contacts" :key="contact">
      {{ contact.first_name }} {{ contact.last_name }}
      <button v-on:click="showContact(contact)">Info</button>
      <button v-on:click="showContactUpdate(contact)">Update</button>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h3>{{ currentContact.first_name }} {{ currentContact.last_name }}</h3>
        <p>Phone: {{ currentContact.phone_number }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <button>Close</button>
      </form>
    </dialog>
    <dialog id="contact-update">
      <form method="dialog">
        <h3>Update contact</h3>
        <p>
          First name:
          <input type="text" v-model="currentContact.first_name" />
        </p>
        <p>
          Last name:
          <input type="text" v-model="currentContact.last_name" />
        </p>
        <p>
          Phone Number:
          <input type="text" v-model="currentContact.phone_number" />
        </p>
        <p>
          Email:
          <input type="text" v-model="currentContact.email" />
        </p>
        <button v-on:click="updateContact(currentContact)">Update Contact</button>
        <button>Close</button>
      </form>
    </dialog>
    <p></p>
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
