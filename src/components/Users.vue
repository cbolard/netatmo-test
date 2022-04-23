<template>


<div class="top-menu">

  <label for="genders">Filter by gender</label>
  <select @change="genderFilter" v-model="selected">
    <option v-for="option in options" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</div>
  <div class="wrapper">
    <div class="flex" v-for="user in users">
      <div class="card">
        <div class="header-card">
          <img class="img-card" v-bind:src="user.picture.large" />
        </div>

        <div class="body-card">
          <h3>{{ user.name.first }} {{ user.name.last }}</h3>
          {{ user.name.title }} {{ user.name.first }} {{ user.name.last }}
          <br />
          {{ user.email }}
          <br />
          {{ user.dob.date }}
          <br />
          {{ user.location.street.number }} {{ user.location.street.name }}
          <br />
          {{ user.phone }}
          <br />
          Password : {{ user.login.password }}
          <br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      users: [],
      req: "?results=10",
      selected: "All",
      options: [
        { text: "Men", value: "?results=10&gender=male" },
        { text: "Women", value: "?results=10&gender=female" },
        { text: "All", value: "?results=10" },
      ],
    };
  },

  mounted() {
    axios.get("https://randomuser.me/api/" + this.req).then((response) => {
      this.users = response.data.results;
    });
  },

  methods: {
    filter(payload) {
      this.users = [];
      this.req = payload;

      axios.get("https://randomuser.me/api/" + this.req).then((response) => {
        this.users = response.data.results;
      });
    },

    genderFilter() {
     this.filter(this.selected);
    },
  },
};
</script>

<style scoped>
.wrapper {
  display: grid;
  grid-auto-flow: row dense;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr;
  gap: 0px 0px;
  grid-template-areas: ". .";
}

* {
  font-size: 1.2rem;
  font-weight: 500;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 2rem;
  color: #38bdf8;
  font-weight: 500;
}

.img-card {
  border-radius: 50%;
  margin: 0 auto;
}
.card {
  margin-bottom: 1rem;
  background: #1e293b;
  color: #bbc5d1;
  border-radius: 30px;
  width: 100%;
  margin: 30px;
  padding: 10px;
}

.header-card {
  padding: 20px;
  text-align: center;
}
.title-card {
  font-size: 1.2rem;
  font-weight: 500;
}
.body-card {
  padding: 20px;
  font-weight: 500;
}

.flex {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: normal;
  align-content: normal;
}

.top-menu {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: flex-end;
    align-items: normal;
    align-content: normal;
    margin-top: 20px;
}

label, select {

  margin-right: 10px;
}


</style>
