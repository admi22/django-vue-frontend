<template>
  <div>
    <h1>Django Cookie Auth</h1>

    <div v-if="this.isAuthenticated">
      <p>You are logged in!</p>
      <button @click="whoami">Who Am I?</button>
      <button @click="logout">Log out</button>
    </div>
    <div v-else>
      <h2>Login</h2>
      <label for="username">Username</label>
      <input type="text" id="username" name="username" v-model="username">
      <br>
      <label for="password">Password</label>
      <input type="text" id="password" name="password" v-model="password">
      <br>
      <button @click="login">Login</button>
    </div>
    <p>{{ this.error }}</p>
  </div>
</template>

<script>

// import axios from "axios"
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      csrf: null,
      username: "",
      password: "",
      error: "",
      isAuthenticated: false
    }
  },
  methods: {
    getCSRF: function () {
      fetch(`${process.env.VUE_APP_API_URL}/csrf/`, {
        credentials: "include",
      }).then((res) => {
        let csrfToken = res.headers.get("X-CSRFToken");
        this.csrf = csrfToken
        console.log({csrfToken});
      }).catch((err) => {
        console.log({err});
      });
    },
    getSession: function () {
      fetch(`${process.env.VUE_APP_API_URL}/session/`, {
        credentials: "include",
      }).then((res) => res.json()).then((data) => {
        console.log({data});
        if (data.isAuthenticated) {
          this.isAuthenticated = true
        } else {
          this.isAuthenticated = false
          this.getCSRF();
        }
      }).catch((err) => {
        console.log({err});
      });
    },
    whoami: function () {
      fetch(`${process.env.VUE_APP_API_URL}/whoami/`, {
        headers: {
          "Content-Type": "application/json",
        },
        credentials: "include",
      }).then((res) => res.json()).then((data) => {
        console.log("You are logged in as: " + data.username);
      }).catch((err) => {
        console.log({err});
      });
    },
    isResponseOk: function (response) {
      if (response.status >= 200 && response.status <= 299) {
        return response.json();
      } else {
        throw Error(response.statusText);
      }
    },
    login: function () {
      fetch(`${process.env.VUE_APP_API_URL}/login/`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "X-CSRFToken": this.csrf,
        },
        credentials: "include",
        body: JSON.stringify({username: this.username, password: this.password}),
      }).then(this.isResponseOk).then((data) => {
        console.log({data});
        this.isAuthenticated = true
        this.username = ""
        this.password = ""
        this.error = ""
      }).catch((err) => {
        console.log({err});
        this.error = "Wrong username or password."
      });
    },
    logout: function () {
      fetch(`${process.env.VUE_APP_API_URL}/logout/`, {
        credentials: "include",
      }).then(this.isResponseOk).then((data) => {
        console.log({data});
        this.isAuthenticated = false
        this.getCSRF();
      }).catch((err) => {
        console.log({err});
      });
    }
  },
  mounted() {
    this.getSession()
  }
}
</script>

<style scoped lang="scss">

</style>
