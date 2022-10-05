<template>
  <header-comp :enableLogin="false"></header-comp> 
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Ažuriranje ličnih podataka
            </h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            <label  for="firstName">Korisničko ime:</label>
            <input class = "center-align" v-model="ulogovaniKorisnik.korisnickoIme" />

            <label for="firstName">Ime:</label>
            <input class = "center-align" v-model="ulogovaniKorisnik.ime" />

            <label for="firstName">Prezime:</label>
            <input class = "center-align" v-model="ulogovaniKorisnik.prezime" />

            <label for="firstName">Lozinka:</label>
            <input class = "center-align" v-model="ulogovaniKorisnik.lozinka" />

            <label for="firstName">Pol: (M / Z) </label>
            <input class = "center-align" v-model="ulogovaniKorisnik.pol" />

            <label for="firstName">Datum rođenja (yyyy-mm-dd):</label>
            <input class = "center-align" v-model="ulogovaniKorisnik.datumRodjenja" />
            <!-- <input class = "center-align" v-model="formatiranDatum" /> -->
             <!-- <input type="text" class="datepicker"  v-model="ulogovaniKorisnik.datumRodjenja"> -->
           
            <a class="waves-effect waves-light btn" v-on:click="azurirajKorisnika()">Ažuriraj podatke</a>
          </div>

          <div class="col s4">
            <opcije-comp :uloga="ulogovaniKorisnik.uloga"></opcije-comp>
          </div>
        </div>
    </div>
  <footer-comp :enableLogin="false"></footer-comp>
</template>

<script>
import FooterComp from '../components/FooterComp.vue';
import HeaderComp from '../components/HeaderComp.vue';
import OpcijeComp from '../components/OpcijeComp.vue'
export default {
    name: "azuriranjeLicnihPodataka",
    components:{
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },

    data: function(){
      return{
        ulogovaniKorisnik: {},
      };
    },
//  + this.$route.query.id
    mounted: function () {
      fetch('http://localhost:8081/api/pregled-licni-podaci/', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          // console.log("Restoran : ", data.restoran.naziv)
          console.log("Success:", data); this.ulogovaniKorisnik = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      

  },

  methods: {
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
        },

        azurirajKorisnika : function(){
            fetch("http://localhost:8081/api/update-licni-podaci",{
            method: "PUT",
            credentials: 'include',
            headers: {
            Accept: "application/json",
            "Content-type": "application/json",
            },  
            body: JSON.stringify(this.ulogovaniKorisnik),
             })
            .then((response) => response.json)
            .then((data) => {
            console.log("Success : " + data);
            this.$router.push("/pregled-licnih-podataka");
            })
            .catch((err) => {
            console.log("Error : " + err);
            alert(err);
            });
        },
  },

  computed:{
    // formatiranDatum: {
    //     get(){
    //         return this.formatDate(this.ulogovaniKorisnik.datumRodjenja)
    //     },
        
    //     set(){
    //         var d = new Date(this.ulogovaniKorisnik.datumRodjenja)
    //         this.ulogovaniKorisnik.datumRodjenja = d;

            
    //     }
    // },
  },

}
</script>

<style scoped>

</style>

