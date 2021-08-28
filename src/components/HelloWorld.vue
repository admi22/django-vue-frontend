<template>
  <div>
    <div>
      <p>Test GET request: {{ this.testGet }}</p>
      <p>Test POST request: {{ this.testPost }}</p>
      <p>CSRF token: {{ this.csrfToken }}</p>
    </div>
<!--    <br>-->
<!--    <button @click="getCSRFToken">GET CSRF token</button>-->
<!--    <br>-->
<!--    <button @click="getPets">GET pets</button>-->
<!--    <br>-->
<!--    <button @click="postPet">POST pet</button>-->
  </div>
</template>

<script>

// import axios from "axios"
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      csrfToken: null,
      testGet: null,
      testPost: null,
    }
  },
  methods: {
    getCsrfToken: async function () {
      if (this.csrfToken === null) {
        const response = await fetch(`${process.env.VUE_APP_API_URL}/csrf`, {
          credentials: 'include',
        });
        const data = await response.json();
        this.csrfToken = data.csrfToken;
      }
      return this.csrfToken;
    },
    testRequest: async function (method) {
      const response = await fetch(`${process.env.VUE_APP_API_URL}/ping`, {
        method: method,
        headers: (
            method === 'POST'
                ? {'X-CSRFToken': await this.getCsrfToken()}
                : {}
        ),
        credentials: 'include',
      });
      const data = await response.json();
      return data.result;
    }
  },
  async mounted() {
    this.testGet = await this.testRequest('GET')
    this.testPost = await this.testRequest('POST')
  }
}
</script>

<style scoped lang="scss">

</style>
