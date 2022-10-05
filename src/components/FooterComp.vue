<template>
    
        <footer class="page-footer grey darken-4">
          <div class="container">
            <div class="row">
              <div class="col l6 s12">
                <h5 class="white-text">Kupi.me</h5>
                <p class="grey-text text-lighten-4">Najbolje mesto za naručivanje hrane koju najviše volite!</p>
              </div>
              <div class="col l4 offset-l2 s12">
                <h5 class="white-text">Linkovi</h5>
                <ul>
                  <li v-if="enableLogin === false " class="disabled"><a class="grey-text text-lighten-3" v-on:click="goToPocetna()">Početna strana</a></li>
                  <li v-else><a class="grey-text text-lighten-3" v-on:click="goToPocetna()">Početna strana</a></li>
                  <li v-if="enableLogin === false " class="disabled"><a class="grey-text text-lighten-3" v-on:click="goToLogin()">Uloguj se</a></li>
                  <li v-else><a class="grey-text text-lighten-3" v-on:click="goToLogin()">Uloguj se</a></li>
                  <li v-if="enableLogin === false " class="disabled"><a class="grey-text text-lighten-3" v-on:click="goToRegister()">Registruj se</a></li>
                  <li v-else><a class="grey-text text-lighten-3" v-on:click="goToRegister()">Registruj se</a></li>
                  <li><a class="grey-text text-lighten-3" v-on:click="logOut()">Izloguj se</a></li>
                </ul>
              </div>
            </div>
          </div>
          <div class="footer-copyright">
            <div class="container">
            © 2022 Aleksandra Arsić, Luka Petrović
            </div>
          </div>
        </footer>
            
</template>

<script>
export default {
    props: ["enableLogin"],
    methods:{
    goToLogin: function () {
      this.$router.push("/login");
    },

    goToRegister: function () {
      this.$router.push("/register");
    },

    goToPocetna: function () {
      this.$router.push("/");
    },

    logOut: function(){
      fetch("http://localhost:8081/api/logout", {
            method: "POST",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
              SameSite:'None'
            },

            })
            .then((response) => response.json)
            .then((data) => {
              alert("Izlogovani ste", this.poruka)
              this.$router.push("/login");
            })
            .catch((err) => {
              console.log("Error : " + err);
              alert(err);
            });
    },

  }
}
</script>

<style scoped>
  li{
    padding: 10px;
  }
</style>