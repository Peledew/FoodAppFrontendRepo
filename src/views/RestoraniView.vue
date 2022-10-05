<template>
  <header-comp></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Restorani</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>

            <ul class="collapsible">
                <li v-for="restoran in restorani" :key="restoran.id">
                    <div class="collapsible-header"><i class="material-icons">restaurant</i>{{restoran.naziv}}</div>
                    <div class="collapsible-body">
                        <span><i class="material-icons">location_on</i>{{restoran.adresaLokacije}}</span><br>
                        <span><i class="material-icons">restaurant</i>{{restoran.tipRestorana}}</span><br>
                        <span><i class="material-icons">restaurant</i>{{restoran.statusRestorana}}</span>
                    </div>
                </li>
            </ul>

          </div>

          <div class="col s4">
            <opcije-comp :uloga="ulogovaniKorisnik.uloga"></opcije-comp>
          </div>
        </div>
    </div>
  <footer-comp></footer-comp>
</template>

<script>
import FooterComp from '../components/FooterComp.vue';
import HeaderComp from '../components/HeaderComp.vue';
import OpcijeComp from '../components/OpcijeComp.vue'
export default {
    name: "RestoraniView",
    components:{
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },
    data: function(){
      return{
        ulogovaniKorisnik:{
            restoran:{},
        },
        restorani: [],
        
      };
    },
    mounted: function () {
      fetch('http://localhost:8081/api/pregled-licni-podaci/', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Success:", data); this.ulogovaniKorisnik = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
        fetch('http://localhost:8081/api/restorani', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Success:", data); this.restorani = data
        
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