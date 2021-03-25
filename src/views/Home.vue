<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p><button v-on:click="createContact()">Create New Contact</button></p>
    <p>
      First Name:
      <input v-model="newFirstName" />
    </p>
    <p>
      Last Name:
      <input v-model="newLastName" />
    </p>
    <p>
      Email:
      <input v-model="newEmail" />
    </p>
    <p>
      Phone Number:
      <input v-model="newPhoneNumber" />
    </p>
    <p>
      Image:
      <input v-model="newImage" />
    </p>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      {{ contact.first_name }} {{ contact.last_name }}
      <hr />
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "This is a Contacts App, all about contacts!",
      contacts: [],
      newFirstName: "",
      newLastName: "",
      newEmail: "",
      newPhoneNumber: "",
      newImage: "",
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("http://localhost:3000/api/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
      });
    },
    createContact: function () {
      console.log("creating contact now")
      var params = {
        first_name: this.newFirstName,
        last_name: this.newLastName,
        email: this.newEmail,
        phone_number: this.newPhoneNumber,
        image: this.newImage,
      };
      axios.post("http://localhost:3000/api/contacts", params).then((response) => {
        console.log(response.data);
        this.contacts = response.data;
        this.contacts.push(response.data);
        this.newFirstName = "";
        this.newLastName = "";
        this.newEmail = "";
        this.newPhoneNumber = "";
        this.newImage = "";
      });
    },
  },
};
</script>
