<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Nalog</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Dobrodošli <b>{{ulogovaniKorisnik.korisnickoIme}}</b>!</p>
          </div>

          <div class="col s4 margin_bottom_10">
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
    name: "nalog",
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

    mounted: function () {
      fetch('http://localhost:8081/api/pregled-licni-podaci/', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {console.log("Success:", data); this.ulogovaniKorisnik = data})
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