<template>
  <div v-if="persons">
    <b-container
      class="bv-example-row border border-primary"
      v-for="person in persons"
      :key="person.email"
    >
      <b-row>
        <b-col cols="auto"><img :src="person.image"/></b-col>
        <b-col>
          <b-row>
            <b-col
              ><strong>{{ person.firstname }}</strong></b-col
            >
          </b-row>
          <b-row>
            <b-col>{{ person.lastname }}</b-col>
          </b-row>
          <b-row>
            <b-col>({{ person.email }})</b-col>
          </b-row>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  created() {
    this.GetPersons();
  },
  data() {
    return {
      persons: null,
    };
  },
  methods: {
    GetPersons() {
      fetch(
        "https://fakerapi.it/api/v1/persons?_quantity=10&_gender=male&_birthday_start=2005-01-01"
      )
        .then((response) => response.json())
        .then((results) => {
          this.persons = results.data;
        });
    },
  },
};
</script>

<style>
.center {
  margin: auto;
  width: 50%;
  border-bottom: 1px solid green;
  padding: 10px;
  text-align: center;
  overflow: hidden;
}

.person {
  text-align: left;
}

.profile-picture {
  float: left;
  margin-right: 15px;
}

img {
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 5px;
  width: 150px;
}
</style>
