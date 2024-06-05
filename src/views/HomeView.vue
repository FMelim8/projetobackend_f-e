<template>
    <LandingPage/>
</template>

<script>
import LandingPage from '../components/LandingPage.vue';



export default {
  name: 'HomeView',
  components: {
    LandingPage
  },
  created: function() {
  var user = this.$route.params.user
  var email = this.$route.params.email
  var password = this.$route.params.password
  var id = this.$route.params.id
},methods: {
  async verifyAccess() {
    var token = localStorage.getItem("token");
    instance.get("/users", {
      //acabar
    })
  }
}
}
</script>
