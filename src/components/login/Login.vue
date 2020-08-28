<template>
  <div>
    <h2>Login</h2>
    <form @submit.prevent="checkForm">
      <div v-if="errors.length">
        <p>
          <b>PLEASE CORRECT THE FOLLOWING ERRORS:</b>
        </p>
        <ul>
          <li v-for="(error, index) in errors" :key="index">{{errors}}</li>
        </ul>
      </div>

      <div>
        <div>
          <label for="email">Email</label>
          <input v-model="user.email" type="email" id="email" name="email" />
        </div>
        <div>
          <input type="submit" value="Register" />
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import EventBus from "../../eventBus.js";
export default {
  name: "Login",
  data: function() {
    return {
      errors: [],
      user: {
        email: ""
      }
    };
  },
  methods: {
    checkForm: function(event) {
      event.preventDefault();
      this.errors = [];
      if (!this.user.email) {
        this.errors.push("Email Required");
      }

      if (!this.errors.length) {
        this.loginUser(this.user);
      }
    }
  },
  loginUser: function(user) {
    this.$http.post(`${process.env.VUE_APP_API_URL}users/login`, user).then(
      response => {
        if (response.body.email) {
          localStorage.loggedIn = "yes";
          localStorage.userEmail = response.body.email;
          EventBus.$emit("$loggedIn");
          this.$router.push({ path: "/" });
        }
      },
      () => {
        console.log(response);
      }
    );
  }
};
</script>

<style>
</style>