<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Naručivanje</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            <p class="left-align fontsize1_25em">Izabrali ste <i>{{korpa.restoran.naziv}}</i></p>
          </div>

          <div class="col s4">
            <opcije-comp :uloga="ulogovaniKorisnik.uloga"></opcije-comp>
          </div>
        </div>

        <div class="row">
          <div class="col s12">
            

                <!-- Modal Structure -->
                <div id="modal1" class="modal bottom-sheet">
                  <div class="modal-content">
                    <h4>Korpa</h4>
                    <table class="centered">
                      <thead>
                        <tr >
                          <th>Naziv</th>
                          <th>Cena</th>
                          <th>Količina</th>
                          <th>Poručena količina</th>
                          <th> Cena porudžbine : <i>{{korpa.ukupnaCenaPorudzbine}}</i></th>
                        </tr>
                      </thead>

                      <tbody>
                        <tr v-for="artikal in pregledajArtikleSortiranje" :key="artikal.id" id="tr_id">
                          <td>{{artikal.nazivArtikla}}</td>
                          <td>{{artikal.cenaArtikla}}</td>
                          <td>
                            {{artikal.kolicinaArtikla}}
                            <span v-if="artikal.tip === 'JELO' ">g</span>
                            <span v-else>ml</span>
                          </td>
                          <td>{{artikal.porucenaKolicina}}</td>
                          <td>
                            <button v-on:click="izmeniKolicinu(artikal.idStavke, artikal.porucenaKolicina + 1)" class="waves-effect waves-light btn blue lighten-1 margin_right_2"><i class="material-icons">add</i></button>
                            <button v-on:click="izmeniKolicinu(artikal.idStavke, artikal.porucenaKolicina - 1)" class="waves-effect waves-light btn red lighten-1 margin_right_6"><i class="material-icons">remove</i></button>
                            <button v-on:click="izbaciStavku(artikal.idStavke)"  class="waves-effect waves-light btn red darken-3">Izbaci artikal</button>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>

                  <div class="modal-footer">
                    <a  class="modal-close waves-effect waves-green btn-flat">Zatvori</a>
                    <a v-on:click="pregledKorpe()" class="waves-effect waves-green btn-flat">Pregled korpe</a>
                  </div>
                </div>
            <table>
              <thead>
                <tr >
                  <th>Naziv</th>
                  <th>Cena</th>
                  <th>Količina</th>
                  <th>Opis</th>
                  <th><!-- Modal Trigger -->
                    <a v-on:click="korpaReload()" id = "modal_triger" class="waves-effect waves-light btn modal-trigger indigo lighten-1" href="#modal1">Korpa</a></th>
                  </tr>
              </thead>

              <tbody>
                <tr v-for="artikal in sortiranaKorpa" :key="artikal.id">
                  <td>{{artikal.naziv}}</td>
                  <td>{{artikal.cena}}</td>
                  <td>
                    {{artikal.kolicina}}
                    <span v-if="artikal.tip === 'JELO' ">g</span>
                    <span v-else>ml</span>
                  </td>
                  <td>{{artikal.opis}}</td>
                  <td><button v-on:click="dodajStavku(artikal.id)" class="waves-effect waves-light btn">Dodaj artikal</button></td>
                  <!-- <td><button   class="waves-effect waves-light btn  red accent-2">Ukloni artikal</button></td> -->
                  <!-- <i class="material-icons right">create</i> -->
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

        korpa : {
          restoran:{
            artikli:[],
          },
          pregledArtikala: [],
          ukupnaCenaPorudzbine:{},
        },
        brNarucenihStavki: "",
        izmenaKolicine: {
          idStavke:"",
          novaKolicina:"",
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

        fetch('http://localhost:8081/api/pregled-korpe', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Korpa:", data); this.korpa = data
          // console.log("BR REDOVA", this.korpa.pregledArtikala.length)
            // console.log("PODACI", this.korpa.restoran.id)
            this.brNarucenihStavki = this.korpa.pregledArtikala.length
            const myElement = document.getElementById("modal_triger");
            myElement.textContent = 'KORPA' + ' [ ' + this.brNarucenihStavki + ' ] '
        })
        .catch((error) => {
          console.error("Error:", error);
        });
        

  },

  computed: {
    pregledajArtikleSortiranje: function () {
      return _.orderBy(this.korpa.pregledArtikala, ['nazivArtikla', 'cenaArtikla', 'porucenaKolicina', 'kolicinaArtikla'])
    },

    sortiranaKorpa : function(){
       return _.orderBy(this.korpa.restoran.artikli , ['naziv', 'cena', 'opis', 'kolicina'])
    },
  },

  methods: {
      dodajStavku : function(id_artikla){
        fetch("http://localhost:8081/api/dodaj-stavku/" + id_artikla, {
            method: "POST",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            
            })
            .then((response) => response.json)
            .then((data) => {
              console.log("Stavka : " + data.toString());
                // this.$router.push("/login");
            })
            .catch((err) => {
              console.log("Error : " + err);
              alert(err);
            });

            
            const myElement = document.getElementById("modal_triger");
            this.brNarucenihStavki++
            myElement.textContent = 'KORPA' + ' [ ' + this.brNarucenihStavki + ' ] '
      },

      korpaReload : function(){
        fetch('http://localhost:8081/api/pregled-korpe', {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Korpa:", data); this.korpa = data
          // console.log("BR REDOVA", this.korpa.pregledArtikala.length)
            // console.log("PODACI", this.korpa.restoran.id)
            this.brNarucenihStavki = this.korpa.pregledArtikala.length
            const myElement = document.getElementById("modal_triger");
            myElement.textContent = 'KORPA' + ' [ ' + this.brNarucenihStavki + ' ] '
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      },

      izbaciStavku: function (stavka_id) {
        

      fetch("http://localhost:8081/api/izbrisi-stavku/" + stavka_id, {
        method: "DELETE",
        credentials: 'include',
      }).then((res) => {
        if (res.ok) {
          this.korpaReload()
        }
      });
    },

    izmeniKolicinu : function(id, porucenaKolicina){
      let x = {
        idStavkeKojaSeMenja : "",
        novaPorucenaKolicina : "",
      }

      x.idStavkeKojaSeMenja = id
      x.novaPorucenaKolicina = porucenaKolicina

      fetch("http://localhost:8081/api/izmeni-kolicinu", {
            method: "PUT",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            
            body: JSON.stringify(x),

            })
            .then((response) => response.json)
            .then((data) => {
              console.log("Stavka : " + data);
              this.korpaReload()
            })
            .catch((err) => {
              console.log("Error : " + err);
              // alert(err);
            });
    },

    pregledKorpe: function () {
      this.$router.push("/pregled-korpe");
    },





  },



}
</script>

<style scoped>
  button{
    max-width: 100%;
  }

  input{
    width: 15% !important;
  }
</style>