<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Naručivanje</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            <ul class="collapsible">
                <li v-for="restoran in restorani" :key="restoran.id">
                    <div class="collapsible-header"><i class="material-icons">restaurant</i>
                      {{restoran.naziv}}
                      <span class = "lean_right text_color_light_gray" v-if="restoran.statusRestorana === 'NE_RADI'"><strong>Zatvoreno</strong></span>
                    </div>
                    <div class="collapsible-body">
                        <div class="row">
                          <div class="col s6 m6">
                            <span>{{restoran.lokacija.adresa}}</span><br>
                            <span>Tip : {{restoran.tipRestorana}}</span><br>
                            <span v-if="restoran.statusRestorana === 'RADI'"><i>Otvoreno</i></span>
                            <span v-else><i>Zatvoreno</i></span>
                          </div>

                          <div class="col s6 m6">
                              <button  v-on:click="kreirajPorudzbinu(restoran.id)" v-if="restoran.statusRestorana === 'RADI' "  class="waves-effect waves-light btn margin_top"><i class="material-icons right">create</i>Kreiraj porudžbinu</button>
                              <button  v-else  class="btn disabled margin_top"><i class="material-icons right">create</i>Kreiraj porudžbinu</button>
                          </div>
                        </div>
                    </div>
                </li>
            </ul>



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
    name: "pregledMenadzeraRestorana",
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

        fetch('http://localhost:8081/api/svi-restorani', {
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
      kreirajPorudzbinu : function(id){
        fetch("http://localhost:8081/api/kreiranje-porudzbine/" + id, {
            method: "POST",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            
            })
            .then((response) => response.json)
            .then((data) => {
              console.log("Success : " + data.toString());
                this.$router.push("/kupovina");
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
  button{
    max-width: 100%;
  }
</style>