<template>
  <header>
    <h3>Header</h3>
    <router-link v-bind:to="'/'">Home</router-link>
    <router-link v-bind:to="'/about'">....About</router-link>
    <router-link v-bind:to="'/register'">....Register</router-link>
    <router-link v-bind:to="'/login'">....LogIn</router-link>
    <a v-if="loggedIn === `yes`" @click.prevent="setLoggedOut" href>LogOut</a>
  </header>
</template>

<script>
import EventBus from "../eventBus.js";
export default {
  name: "MyHeader",
  data: function() {
    return {
      loggedIn: "no",
      userEmail: ""
    };
  },
  methods: {
    setLoggedout: function() {
      if (response.body.email) {
        localStorage.loggedIn = "no";
        localStorage.userEmail = null;
        EventBus.$emit("$loggedIn");
        this.$router.push({ path: "/login" });
      }
    },
    setLoggedIn: function() {
      this.loggedIn = localStorage.loggedIn;
      this.userEmail = localStorage.userEmail;
    }
  },
  mounted() {
    this.loggedIn = localStorage.loggedIn;
    this.userEmail = localStorage.userEmail;
    EventBus.$on("$userLogged", this.setLoggedIn);
  }
};
</script>

<style>
.router-link-exact-active {
  color: green;
}
</style>