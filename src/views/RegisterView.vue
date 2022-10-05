<template>
  <header-comp></header-comp>  
  <div class="container">
    <h1 class = "left-align center">Unesite podatke:</h1>
      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="noviKorisnik.korisnickoIme" id="first_name" type="text" class="validate" autofocus>
          <label for="first_name">Korisničko ime</label>
        </div>
      </div>
      
      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="noviKorisnik.lozinka" id="password" type="password" class="validate">
          <label for="password">Lozinka</label>
        </div>
      </div>

      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="noviKorisnik.ime" id="name" type="text" class="validate">
          <label for="name">Ime</label>
        </div>
      </div>

      <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="noviKorisnik.prezime" id="last_name" type="text" class="validate">
          <label for="last_name">Prezime</label>
        </div>
      </div>

        <div class="row">
            <div class="col s3 offset-s2">
              <p>Pol:</p>    
            </div>

            <div class="col s2">
                <p> 
                    <label>
                        <input v-model="noviKorisnik.pol" class="with-gap" name="group1" type="radio" value="M" checked/>
                        <span>Muško</span>
                    </label>
                </p>    
            </div>

            <div class="col s2">
                <p>
                    <label>
                        <input v-model="noviKorisnik.pol" class="with-gap" name="group1" type="radio" value="Z" />
                        <span>Žensko</span>
                    </label>
                </p>
            </div>
        </div>

        <!-- <div class="row">
            <div class="input-field col s6 offset-s3">
                <p class = "left-align">Datum rođenja</p>
                <input v-model="noviKorisnik.datumRodjenja" type="text" class="datepicker">
            </div>
        </div> -->

        <div class="row">
        <div class="input-field col s6 offset-s3">
          <input v-model="noviKorisnik.datumRodjenja" id="last_name" type="text" class="validate">
          <label for="last_name">Datum rođenja</label>
        </div>
      </div>

      <div class="row">
        <div class="col s12"> 
            <button v-on:click = "registrujSe()" class="margin2_5 btn waves-effect waves-light">Registruj se
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
// import M from 'materialize-css'
// var instance = M.Datepicker.getInstance(elem)
export default {
    name: "register",
    components:{
    HeaderComp,
    FooterComp
  },

    data: function(){
      return{
        noviKorisnik: {
          korisnickoIme: "",
          lozinka: "",
          ime: "",
          prezime: "",
          pol: "M",
          datumRodjenja: ""
        },
      };
    },

    methods: {
        registrujSe : function(){
          fetch("http://localhost:8081/api/register", {
            method: "POST",
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            
            body: JSON.stringify(this.noviKorisnik),
            })
            .then((response) => response.json)
            .then((data) => {
              console.log("Success : " + data.toString());
              console.log(this.noviKorisnik)
              alert("Korisnik uspesno registrovan! : " + this.noviKorisnik.korisnickoIme + " Datum: " + this.noviKorisnik.datumRodjenja)
                this.$router.push("/login");
            })
            .catch((err) => {
              console.log("Error : " + err);
              alert(err);
            });
        },

        formatDate : function(date) {
            var d = new Date(date),
                month = '' + (d.getMonth() + 1),
                day = '' + d.getDate(),
                year = d.getFullYear();

            if (month.length < 2) 
                month = '0' + month;
            if (day.length < 2) 
                day = '0' + day;

            return [year, month, day].join('-');
        }

       

    },

    
}
</script>

<style scoped>
  .margin15_10{
    margin: 15% 10%;
  }

  .margin2_5{
    margin: 2% 5%;
  }
</style>