<template>
  <div class="home">
    <h1>{{ message }}</h1>
      <div><p>first_name: <input type="text" v-model="newContact.first_name"></p>
      <p>last_name: <input type="text" v-model="newContact.last_name"></p>
      <p>email: <input type="text" v-model="newContact.email"></p>
      <p>phone_number: <input type="text" v-model="newContact.phone_number"></p>
      <p>image: <input type="text" v-model="newContact.image"></p>
      <button v-on:click="createContact()">Add New Contact</button>
      </div>
      <p></p>
      <hr>
      <p></p>
      <div v-for="contact in contacts">
      <p>First Name: {{ contact.first_name }}</p>
      <p>Last Name: {{ contact.last_name }}</p>
      <p>Email: {{ contact.email }}</p>
      <p>Phone Number: {{ contact.phone_number }}</p>
      <img v-bind:src="contact.image">
      <p><button v-on:click="showContact(contact)">See more info</button></p>
      <hr>
      </div>
      <dialog id="contact-details">
        <form method="dialog">
        <p>first name: <input type="text" v-model="currentContact.first_name"></p>
        <p>last name: <input type="text" v-model="currentContact.last_name"></p>
        <p>email: <input type="text" v-model="currentContact.email"></p>
        <p>phone number: <input type="text" v-model="currentContact.phone_number"></p>
        <p>image: <input type="text" v-model="currentContact.image"></p>
        <button v-on:click="updateContact(currentContact)">Update Contact info</button>
        <button>Close</button>
        <button v-on:click="destroyContact(currentContact)">Delete</button>
      </form>
    </dialog>
  </div>
</template>
<style>
  img{
    width: 200px;
  }
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Simple Contacts!",
      contacts: [],
      newContact: {},
      currentContact: {},
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
    createContact: function() {
      console.log("create contact");
      // var newContact = {
        // first_name: this.newContact.first_name,
        // last_name: this.newContact.last_name,
        // email: this.newContact.email,
        // phone_number: this.newContact.phone_number,
        // image: this.newContact.image,
      // };
      axios.post("http://localhost:3000/contacts", this.newContact).then(response => {
        console.log(response.data);
        this.contacts.push(response.data);
        this.newContact={};
      });
    },
    showContact: function(theContact) {
      console.log(theContact);
      this.currentContact = theContact;
      // open the dialog box
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function(theContact) {
      console.log("updating contact");
      axios.patch("http://localhost:3000/contacts/${theContact.id}", theContact).then(response => {
        console.log(response.data);
      });
    },
    destroyContact: function(theContact) {
      axios.delete("http://localhost:3000/contacts/${theContact.id}").then(response => {
        console.log(response.data);
        var index = this.contacts.indexOf(theContact);
        this.contacts.splice(index, 1);
      });
    }
  };
</script>
