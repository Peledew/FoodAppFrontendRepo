<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s12 m8">
            <h1 class="left-align">Pregled porudžbina kupca</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            
            <porudzbina-comp
            v-for="porudzbina in porudzbineKupca"
            :key="porudzbina.uuid"
            :porudzbina="porudzbina"
            :kupac="ulogovaniKorisnik"
            :restoran=null
            >
            </porudzbina-comp>
          </div>

          <div class="col s12 m4">
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
import PorudzbinaComp from '../components/PorudzbinaComp.vue'
export default {
    name: "pregledMenadzeraRestorana",
    components:{
    HeaderComp,
    FooterComp,
    OpcijeComp,
    PorudzbinaComp,
  },

    data: function(){
      return{
        ulogovaniKorisnik:{},
        porudzbineKupca: {
          stavkePorudzbine:{},
        }
      };
    },

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

        fetch('http://localhost:8081/api/kupac-pregled-porudzbina', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Porudzbine:", data); this.porudzbineKupca = data
        
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