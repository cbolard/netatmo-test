<template>

  <div class="top-menu">
    <button @click="reset" class="btn-primary">Reset board</button>
    <div>
    <label for="genders">Filter by gender</label>
    <select @change="genderFilter" v-model="selected">
      <option v-for="option in options" :value="option.value">
        {{ option.text }}
      </option>
    </select>
    </div>
  </div>

<div class="container"  >
  
  <div class="card" v-for="profile in profiles">
    <div class="card__header">
      <img v-bind:src="profile.image" alt="card__image" class="card__image" width="600">
    </div>
    <div class="card__body">
      <span class="tag tag-blue">   {{ profile.phone }}</span>
      <h4> {{profile.nom }}</h4>
      <p>{{ profile.email }}
            <br />
           {{ profile.date }}
            <br />
            {{ profile.adresse }}
            <br /></p>
    </div>
    <div class="card__footer">
      <div class="user">
        <img v-bind:src="profile.thumbnail" alt="user__image" class="user__image">
        <div class="user__info">
          <h5>{{profile.nom }}</h5>
          <small>
            Password : {{ profile.password }}
            <br /></small>
        </div>
      </div>
    </div>
  </div>

</div>

  <div class="down-menu">
     <button @click="addTenProfiles" class="btn-primary">Voir plus</button>
  </div>






</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      users: [],
      profiles: [],
      dob: [],
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
      
      this.getProfile(this.users)
    });
  },

  methods: {
    genderFilter() {
      this.profiles = [];
      this.dob = [];

      this.req = this.selected;
      axios.get("https://randomuser.me/api/" + this.req).then((response) => {
        this.users = response.data.results;   
        this.getProfile(this.users)
     
      });


    },

    getProfile(resp){

      this.getDate(resp)
      for (let i = 0; i < resp.length; i++) {
        this.profiles.push({
          thumbnail: resp[i].picture.thumbnail,
          image: resp[i].picture.large,
          gender: resp[i].gender,
          nom : resp[i].name.first + " " +  resp[i].name.last,
          email :resp[i].email,
          date: this.dob[i],
          adresse: resp[i].location.street.number + " " + resp[i].location.street.name,
          phone: resp[i].phone,
          password: resp[i].login.password
        })

      }
    },

    getDate(resp){
      for (let i = 0; i < resp.length; i++) {
        let dob = resp[i].dob.date.split("T")[0];
        let date = dob.split("-");
        const mois = [
          "Janvier",
          "Février",
          "Mars",
          "Avril",
          "Mai",
          "Juin",
          "Juillet",
          "Août",
          "Septembre",
          "Octobre",
          "Novembre",
          "Décembre",
        ];
        let date_format = date[2] + " " + mois[date[1] - 1] + " " + date[0];
        this.dob.push(date_format);
      }
    },

    reset(){
      this.profiles = [];
      this.dob = [];

    },

    addTenProfiles() {
      axios.get("https://randomuser.me/api/" + this.req).then((response) => {
        this.users = response.data.results;
        this.getProfile(this.users)
      });
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Quicksand:wght@300..700&display=swap");

*,
*::before,
*::after {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

.btn-primary{
  background-color: #0f172a;
  color: white;
  border: none;
  padding: 5px 20px;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
}

.down-menu{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

select{
  width: 100%;
  padding: 5px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.top-menu{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}

body {
  font-family: "Quicksand", sans-serif;
  display: grid;
  place-items: center;
  height: 100vh;
  background: #7f7fd5;
  background: linear-gradient(to right, #91eae4, #86a8e7, #7f7fd5);
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 1200px;
  margin-block: 2rem;
  gap: 2rem;
}

img {
  max-width: 100%;
  display: block;
  object-fit: cover;
}

.card {
  width: 300px;
  display: flex;
  flex-direction: column;
  width: clamp(20rem, calc(20rem + 2vw), 22rem);
  overflow: hidden;
  box-shadow: 0 .1rem 1rem rgba(0, 0, 0, 0.1);
  border-radius: 1em;
  background: #ECE9E6;
background: linear-gradient(to right, #FFFFFF, #ECE9E6);

}



.card__body {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: .5rem;
}


.tag {
  align-self: flex-start;
  padding: .25em .75em;
  border-radius: 1em;
  font-size: .75rem;
}

.tag + .tag {
  margin-left: .5em;
}

.tag-blue {
  background: #0f172a;
  color: #fafafa;
}

.tag-brown {
  background: #D1913C;
background: linear-gradient(to bottom, #FFD194, #D1913C);
  color: #fafafa;
}

.tag-red {
  background: #cb2d3e;
background: linear-gradient(to bottom, #ef473a, #cb2d3e);
  color: #fafafa;
}

.card__body h4 {
  font-size: 1.5rem;
  text-transform: capitalize;
}

.card__footer {
  display: flex;
  padding: 1rem;
  margin-top: auto;
}

.user {
  display: flex;
  gap: .5rem;
}

.user__image {
  border-radius: 50%;
}

.user__info > small {
  color: #666;
}
</style>
