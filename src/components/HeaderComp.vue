<template>
    <header>
      <div class="navbar-fixed">
        <nav class = "grey darken-4">
          <div class="nav-wrapper">
            <a href="/" class="brand-logo">
              <i class="material-icons fontsize1_25em">shopping_cart</i>
              Kupi.me
            </a>

              <a href="#" data-target="mobile-demo" class="sidenav-trigger"><i class="material-icons">menu</i></a>
              <ul class="right hide-on-med-and-down">
                <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToPocetna()"> <span class ="fontsize1_25em">Početna strana</span></a></li>
                <li v-else><a v-on:click="goToPocetna()"> <span class ="fontsize1_25em">Početna strana</span></a></li>
                <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToLogin()"><span class ="fontsize1_25em">Uloguj se</span></a></li>
                <li v-else><a v-on:click="goToLogin()"><span class ="fontsize1_25em">Uloguj se</span></a></li>
                <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToRegister()"><span class ="fontsize1_25em">Registruj se</span></a></li>
                <li v-else><a v-on:click="goToRegister()"><span class ="fontsize1_25em">Registruj se</span></a></li>
                <li><a v-on:click="logOut()"><span class ="fontsize1_25em">Izloguj se</span></a></li>
              </ul>
          </div>
        </nav>
      </div>

      <ul class="sidenav" id="mobile-demo">
        <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToPocetna()">Početna strana</a></li>
        <li v-else><a v-on:click="goToPocetna()">Početna strana</a></li>
        <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToLogin()">Uloguj se</a></li>
        <li v-else><a v-on:click="goToLogin()">Uloguj se</a></li>
        <li v-if="enableLogin === false " class="disabled"><a v-on:click="goToRegister()">Registruj se</a></li>
        <li v-else><a v-on:click="goToRegister()">Registruj se</a></li>
        <li><a  v-on:click="logOut()">Izloguj se</a></li>
      </ul>
    </header>

</template>

<script>
import M from 'materialize-css'
export default {
    name:'HeaderComp',
    props: ["enableLogin"],
     mounted(){
    M.AutoInit()
    },

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
nav{
    background-color: transparent;
    box-shadow: none;
}

li{
  font-weight: bold;
  padding: 0 20px;
}

header{
  text-align: justify;
}
</style>