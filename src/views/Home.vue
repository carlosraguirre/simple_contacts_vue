<template>
  <div class="home">
    <h1>{{ message }}</h1>
      <div><p>First Name: <input type="text" v-model="newContactParams.first_name"></p>
      <p>Last Name: <input type="text" v-model="newContactParams.last_name"></p>
      <p>Email: <input type="text" v-model="newContactParams.email"></p>
      <p>Phone Number: <input type="text" v-model="newContactParams.phone_number"></p>
      <p>Image: <input type="text" v-model="newContactParams.image"></p>
      <button v-on:click="contactsCreate()">Create Contact</button>
      </div>
      <p></p>
      <hr>
      <p></p>
      <div v-for="contact in contacts">
      <p>First Name: {{ contact.first_name }}</p>
      <p>Last Name: {{ contact.last_name }}</p>
      <p>Email: {{ contact.email }}</p>
      <p>Phone Number: {{ contact.phone_number }}</p>
      <p>Image: {{ contact.image }}</p>
      <img v-bind:src="contact.image">
      <hr>
      </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Simple Contacts!",
      contacts: [],
      newContactParams: {},
    };
  },
  created: function () {
    this.contactsIndex();
  },
  methods: {
    contactsIndex: function () {
      console.log("show all contacts");
      axios.get("http://localhost:3000/contacts").then(response => {
        console.log(response.data);
        this.contacts = response.data;
      });
    },
  },
    contactsCreate: function() {
      console.log("create contact");
      var contactParams = {
        first_name: this.newContactParams.first_name,
        last_name: this.newContactParams.last_name,
        email: this.newContactParams.email,
        phone_number: this.newContactParams.phone_number,
        image: this.newContactParams.image,
      };
      axios.post("http://localhost:3000/contacts", contactParams).then(response => {
        console.log(response.data);
        this.contacts.push(response.data);
        this.newContactParams={};
      });
    },
    
  };
</script>
