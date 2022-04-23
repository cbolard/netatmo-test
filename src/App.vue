<script setup>
import Users from "./components/Users.vue";
import Navbar from "./components/Navbar.vue";
</script>

<template>
  <div>
    <Navbar />

      <Users />
    </div>
</template>

<style>
@import "./assets/base.css";


#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;

}


header {
  line-height: 1.5;
}



</style>
