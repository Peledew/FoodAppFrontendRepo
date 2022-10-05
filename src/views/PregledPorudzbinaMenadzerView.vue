<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pregled porudžbina menadžerovog restorana</h1>
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
            <!-- <porudzbina-comp
            v-for="porudzbina in porudzbineMenadzerovogRestorana"
            :key="porudzbina.uuid"
            :porudzbina="porudzbina"
            :kupac=null
            :restoran="ulogovaniKorisnik.restoran"
            >
            </porudzbina-comp> -->

          <table class="striped z-depth-1" v-for="porudzbina in porudzbineMenadzerovogRestorana" :key="porudzbina.uuid">
            <tbody>
              <tr>
                <td><b>UUID:</b></td>
                <td>{{porudzbina.uuid}}</td>
              </tr>
              <tr>
                <td><b>Stavke porudžbine:</b></td>
                <td>
                    <ul class = "list_style_type_none">
                        <li  v-for = "stavka in porudzbina.stavkePorudzbine" :key ="stavka.artikal.naziv">{{stavka.porucenaKolicina}}x, {{stavka.artikal.naziv}}, <i>{{stavka.artikal.cena}}din.</i> </li>
                    </ul>
                </td>
              </tr>
              <tr>
                <td><b>Restoran:</b></td>
                <td>{{ulogovaniKorisnik.restoran.naziv}}</td>
              </tr>
              <tr>
                <td><b>Datum i vreme porudzbine:</b></td>
                <td>{{porudzbina.datumIVreme}}</td>
              </tr>
              <tr>
                <td><b>Cena:</b></td>
                <td>{{porudzbina.cena}}</td>
              </tr>
              <tr>
                <td><b>Status:</b></td>
                <td>{{porudzbina.status}}</td>
              </tr>
              <tr>
                <td><b>Promeni status:</b></td>
                <td><button class="waves-effect waves-light btn" v-on:click="izmeniStatus(porudzbina.uuid)">Izmeni</button></td>
              </tr>
            </tbody>
         </table>

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
        ulogovaniKorisnik:{
            restoran:{},
        },
        porudzbineMenadzerovogRestorana: {
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
          console.log("Success:", data); this.ulogovaniKorisnik = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });

        fetch('http://localhost:8081/api/menadzer-pregled-porudzbina', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Porudzbine:", data); this.porudzbineMenadzerovogRestorana = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      

  },

  methods: {
    izmeniStatus : function(uuid){
            fetch("http://localhost:8081/api/menadzer-pregled-porudzbina/" + uuid,{
            method: "PUT",
            credentials: 'include',
            headers: {
            Accept: "application/json",
            "Content-type": "application/json",
            },  

             })
            .then((response) => response.json)
            .then((data) => {
            console.log("Success : " + data);
            window.location.reload()
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
table{
  margin-bottom: 8%;
}
</style>