<template>
  <header-comp :enableLogin="false"></header-comp> 
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Dodavanje artikla
            </h1>
            <div class="divider"></div>

            <label  for="naziv">Naziv artikla:</label>
            <input class = "center-align" v-model="noviArtikal.naziv" />

            <label for="cena">Cena:</label>
            <input class = "center-align" v-model="noviArtikal.cena" />

            <label for="kolicina">Količina:</label>
            <input class = "center-align" v-model="noviArtikal.kolicina" />

            <label for="opis">Opis:</label>
            <input class = "center-align" v-model="noviArtikal.opis" />

            <label for="tip">Tip: (JELO / PICE) </label>
            <input class = "center-align" v-model="noviArtikal.tip" />

            
            <div class="file-field input-field">
                <div class="btn">
                    <span>File</span>
                    <input  type="file"  @change="previewFiles">
                </div>

                <div class="file-path-wrapper">
                    <input class="file-path validate" type="text">
                </div>
            </div>

            <a class="waves-effect waves-light btn"  v-on:click="dodajArtikal()">Dodaj artikal</a>
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
        ulogovaniKorisnik:{},
        noviArtikal: {},
      };
    },

    mounted: function () {
    //   Dobavljanje korisnika zbog uloge
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
        dodajArtikal : function(){
            alert(this.noviArtikal.url)
            fetch("http://localhost:8081/api/dodavanje-novoga-artikla",{
            method: "POST",
            credentials: 'include',
            headers: {
            Accept: "application/json",
            "Content-type": "application/json",
            },  
            body: JSON.stringify(this.noviArtikal),
             })
            .then((response) => response.json)
            .then((data) => {
            console.log("Novi artikal : " + data);
            this.$router.push("/pregled-restorana-menadzera");
            })
            .catch((err) => {
            console.log("Error : " + err);
            alert(err);
            });
      
        },
        
        previewFiles(event) {
        this.noviArtikal.url = event.target.files[0].name
        },


  },



}
</script>

<style scoped>

</style>

