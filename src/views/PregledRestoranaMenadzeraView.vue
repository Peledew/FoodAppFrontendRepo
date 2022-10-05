<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pregled restorana menadžera</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            <!-- Prikaz restorana --> 
            <h2 class="left-align">Restoran</h2>
            <table>
              <tbody>
                <tr>
                  <td><b>Ime restorana:</b></td>
                  <td>{{restoranIPorudzbine.menadzerovRestoran.naziv}}</td>
                </tr>
                <tr>
                  <td><b>Tip restorana:</b></td>
                  <td>{{restoranIPorudzbine.menadzerovRestoran.tipRestorana}}</td>
                </tr>
                <tr>
                  <td><b>Lokacija:</b></td>
                  <td>{{restoranIPorudzbine.menadzerovRestoran.lokacija.adresa}}</td>
                </tr>
              </tbody>
            </table>

            <div class="col s4">
                <p class="left-align"><b>Artikli:</b></p>
            </div>

            <div class="col s8">
                <ul class = "list_style_type_none left-align margin_left_8_5">
                    <li  v-for = "artikal in restoranIPorudzbine.menadzerovRestoran.artikli" :key ="artikal.naziv">
                   <p text-align="center"> {{artikal.naziv}}</p>
                    <button  class = "delBtn" v-on:click="ukloniArtikal(artikal)" >
                      Ukloni
                    </button>

                    <button  class = "delBtn" v-on:click="azurirajArtikal(artikal.id)" >
                      Azuriraj 
                    </button>
                      <hr class="solid">
                    </li>
                </ul>
            </div>
            
            <div class="row">
                <div class="col s12">
                    <h2 class="left-align">Porudzbine</h2>     
                    <porudzbina-comp
                    v-for="porudzbina in restoranIPorudzbine.porudzbineMenadzerovogRestorana"
                    :key="porudzbina.uuid"
                    :porudzbina="porudzbina"
                    :kupac=null
                    :restoran="restoranIPorudzbine.menadzerovRestoran"
                    >
                    </porudzbina-comp>
                </div>  
            </div>
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
        ulogovaniKorisnik: {},
        restoranIPorudzbine: {
            menadzerovRestoran:{
                lokacija:{},
                artikli:{},
            },
            porudzbineMenadzerovogRestorana:{
                stavkePorudzbine: {},
            },
        },
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

        fetch('http://localhost:8081/api/menadzer-pregled', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Podaci", data.menadzerovRestoran.lokacija.adresa)
          console.log("Restoran i porudzbine:", data); this.restoranIPorudzbine = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      

  },

  methods: {

     ukloniArtikal: function (artikal) {
      this.$router.push("/ukloni?id=" + artikal.id);
    },

    azurirajArtikal : function(id_artikla){
      document.cookie = "artikal_id=" + id_artikla;
      // console.log(id_artikla)
      this.$router.push("/azuriranje-artikla");
    }

  },

}
</script>

<style scoped>

</style>