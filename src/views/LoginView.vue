<template>
  <header-comp></header-comp>  
  <div class="margin15_10">
    <h1 class = "left-align center">Unesite podatke:</h1>
      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="ulogovaniKorisnik.korisnickoIme" id="first_name" type="text" class="validate" autofocus>
          <label for="first_name">Korisničko ime</label>
        </div>
      </div>
      
      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="ulogovaniKorisnik.lozinka" id="password" type="password" class="validate">
          <label for="password">Lozinka</label>
        </div>
      </div>

      <div class="row">
        <div class="col s12"> 
            <button v-on:click = "ulogujSe()" class="margin2_5 btn waves-effect waves-light">Uloguj se
                <i class="material-icons right">send</i>
            </button>
        </div>
      </div>
  </div>
  <footer-comp></footer-comp>
</template>

<script>
import FooterComp from '../components/FooterComp.vue';
import HeaderComp from '../components/HeaderComp.vue';
export default {
    name: "login",
    components:{
    HeaderComp,
    FooterComp
  },

    data: function(){
      return{
        ulogovaniKorisnik: {
          korisnickoIme: "",
          lozinka: "",
        },
      };
    },

    methods: {
        ulogujSe : function(){
          fetch("http://localhost:8081/api/login", {
            method: "POST",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            body: JSON.stringify(this.ulogovaniKorisnik),
            })
            .then((response) => response.json)
            .then((data) => {
              console.log("Success : " + data.toString());
              console.log(this.ulogovaniKorisnik)
              if(this.ulogovaniKorisnik.korisnickoIme != "")
                  this.$router.push("/nalog");
              else
                alert("Morate uneti trazene podatke!")
            })
            .catch((err) => {
              console.log("Error : " + err);
              alert(err);
            });
        },

        


    },
}
</script>

<style scoped>
  
</style>