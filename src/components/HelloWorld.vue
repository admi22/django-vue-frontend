<template>
  <div>
    <button @click="requestTestApi">requestTestApi</button>
  </div>
</template>

<script>

import axios from "axios"
export default {
  name: 'HelloWorld',
  methods: {
    requestTestApi: function () {
      axios.get(`${process.env.VUE_APP_API_URL}/test/`, {
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
