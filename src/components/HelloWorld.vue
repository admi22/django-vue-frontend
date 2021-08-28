<template>
  <div>
    <button @click="getPets">GET pets</button>
    <button @click="postPet">POST pet</button>
  </div>
</template>

<script>

import axios from "axios"
export default {
  name: 'HelloWorld',
  methods: {
    getPets: function () {
      axios.get(`${process.env.VUE_APP_API_URL}/pets/`, {
        // headers: {
        //   'X-CSRFToken': csrftoken,
        //   'Cache-Control': 'no-cache',
        //   'Pragma': 'no-cache',
        // },
        // withCredentials: true
      }).then((res) => {
        console.log(res)
      }).catch((err) => {
        console.log(err)
      })
    },
    postPet: function () {
      let data = {}
      axios.post(`${process.env.VUE_APP_API_URL}/pets/`, data, {
        // headers: {
        //   'X-CSRFToken': csrftoken,
        //   'Cache-Control': 'no-cache',
        //   'Pragma': 'no-cache',
        // },
        // withCredentials: true
      }).then((res) => {
        console.log(res)
      }).catch((err) => {
        console.log(err)
      })
    }
  },
}
</script>

<style scoped lang="scss">

</style>
