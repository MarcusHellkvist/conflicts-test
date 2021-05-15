<template>
  <div class="home">
    <b-form-input
      v-model="username"
      placeholder="Enter your name"
    ></b-form-input>
    <b-form-input
      v-model="password"
      placeholder="Enter your password"
    ></b-form-input>
    <div class="mt-2">Value: {{ username }}</div>
    <b-button v-on:click="getAllUsers">Add data</b-button>
  </div>
</template>

<script>
import { db } from "../firebase/db";

export default {
  created() {
    db.collection("users")
      .doc("ASaVmKyC3qJOyt3hiwMK")
      .onSnapshot((doc) => {
        console.log("Current data: ", doc.data());
      });
  },
  data() {
    return {
      username: "",
      password: "",
      documents: [],
    };
  },
  methods: {
    getAllUsers() {
      db.collection("users")
        .get()
        .then((querySnapshot) => {
          const documents = querySnapshot.docs.map((doc) => doc.data());
          documents.forEach((element) => {
            console.log(element.name);
          });
        });
    },
    add() {
      db.collection("users").add({
        name: this.username,
        password: this.password,
      });
    },
    run() {
      this.documents.forEach((element) => {
        console.log(element);
      });
    },
  },
  name: "Home",
  firestore: {
    documents: db.collection("documents"),
    users: db.collection("users"),
  },
};
</script>
