<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pregled svih korisnika</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
          </div>

          <div class="col s4">
            <opcije-comp :uloga="ulogovaniKorisnik.uloga"></opcije-comp>
          </div>
        </div>

        <div class="row">
          <div class="col s12">
            <korisnik-card-comp
          v-for="korisnik in sviKorisnici"
          :key="korisnik.id"
          :korisnik="korisnik"
          >
          </korisnik-card-comp>
          </div>
        </div>
    </div>
  <footer-comp :enableLogin="false"></footer-comp>
</template>

<script>
import FooterComp from '../components/FooterComp.vue';
import HeaderComp from '../components/HeaderComp.vue';
import OpcijeComp from '../components/OpcijeComp.vue';
import KorisnikCardComp from '../components/KorisnikCardComp.vue';
export default {
    name: "nalog",
    components:{
    HeaderComp,
    FooterComp,
    OpcijeComp,
    KorisnikCardComp
  },

    data: function(){
      return{
        ulogovaniKorisnik: {},
        sviKorisnici : [],
      };
    },

    //Pri ucitavanju dobavimo ulogovanog korisnika da bi prikazali njegov username
    //Iskoristimo to da bismo videli i njegovu ulogu i nju prosledili komponenti koja pikazuje opcije
    //Potom saljemo fetch zahtev kako bismo dobavili sve korisnike
    mounted: function () {
      fetch('http://localhost:8081/api/pregled-licni-podaci/', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          // console.log("Restoran : ", data.restoran.naziv)
          console.log("Jeste:", data); this.ulogovaniKorisnik = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      
      fetch('http://localhost:8081/api/admin-pregled', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          // console.log("Restoran : ", data.restoran.naziv)
          console.log("Success:", data); this.sviKorisnici = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });

  },


  methods: {

  },

}
</script>

<style scoped>

</style>