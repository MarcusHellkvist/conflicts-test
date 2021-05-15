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
    <b-button v-on:click="getProducts">Add data</b-button>
    <ul v-if="categories">
      <li v-for="category in categories" :key="category.name">
        {{ category.name }}
      </li>
    </ul>

    <div>
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-1"
          label="Product name"
          label-for="input-1"
          description="Add a product name"
        >
          <b-form-input
            id="input-1"
            v-model="newProduct.name"
            placeholder="Enter product name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Price:" label-for="input-2">
          <b-form-input
            id="input-2"
            v-model="newProduct.price"
            placeholder="Enter product price"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-3" label="Category:" label-for="input-3">
          <b-form-select
            id="input-3"
            v-model="newProduct.category"
            :options="productCategories"
            required
          ></b-form-select>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
import { db } from "../firebase/db";

export default {
  created() {
    this.getCategories();
  },
  data() {
    return {
      username: "",
      password: "",
      documents: [],
      categories: [],
      newProduct: {
        name: "",
        price: "",
        category: null,
      },
      show: true,
      productCategories: [{ text: "Select One", value: null }],
    };
  },
  methods: {
    getCategories() {
      console.log("fetching categories");
      db.collection("categories")
        .get()
        .then((querySnapshot) => {
          const documents = querySnapshot.docs.map((doc) => doc.data());
          console.log(documents[0]);
          documents.forEach((element) => {
            this.productCategories.push(element.name);
          });
        });
    },
    getProducts() {
      db.collection("categories")
        .doc("7HLqAwdZQ3ohi0uydxTp")
        .collection("products")
        .get()
        .then((querySnapshot) => {
          const categories = querySnapshot.docs.map((doc) => doc.data());
          categories.forEach((element) => {
            console.log(`Product: ${element.name} - Price: ${element.price}`);
          });
        });
    },
    onSubmit(event) {
      event.preventDefault();
      db.collection("categories")
        .doc("7HLqAwdZQ3ohi0uydxTp")
        .collection("products")
        .add({
          name: this.newProduct.name,
          price: this.newProduct.price,
        });
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
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
    categories: db.collection("categories"),
  },
};
</script>
