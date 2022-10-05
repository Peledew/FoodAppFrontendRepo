<template>
  <header-comp :enableLogin="false"></header-comp> 
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Ažuriranje artikla
            </h1>
            <div class="divider"></div>

            <label  for="naziv">Naziv artikla:</label>
            <input class = "center-align" v-model="trazeniArtikal.naziv" />

            <label for="cena">Cena:</label>
            <input class = "center-align" v-model="trazeniArtikal.cena" />

            <label for="kolicina">Količina:</label>
            <input class = "center-align" v-model="trazeniArtikal.kolicina" />

            <label for="opis">Opis:</label>
            <input class = "center-align" v-model="trazeniArtikal.opis" />

            <label for="tip">Tip: (JELO / PICE) </label>
            <input class = "center-align" v-model="trazeniArtikal.tip" />

            <div class="file-field input-field">
                <div class="btn">
                    <span>File</span>
                    <input  type="file"  @change="previewFiles">
                </div>

                <div class="file-path-wrapper">
                    <input class="file-path validate" type="text">
                </div>
            </div>
          
            <a class="waves-effect waves-light btn"  v-on:click="azurirajArtikal()">Ažuriraj podatke</a>
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
        trazeniArtikal: {},
        idArtikla:{},
      };
    },

    mounted: function () {
        let x = document.cookie
        this.idArtikla = x.substring(11)
        // alert(this.idArtikla)
        
      
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

        // Dobavljanje informacija o artiklu
        fetch('http://localhost:8081/api/prikaz-artikla/' + this.idArtikla, {
        credentials: 'include'
      })
        .then(response => response.json())
        .then(data => {
          console.log("Success:", data);
           this.trazeniArtikal = data
            document.cookie = "artikal_id=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
        })
        .catch((error) => {
          console.error("Error:", error);
        });
      

  },
 
  methods: {
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



        azurirajArtikal : function(){
            fetch("http://localhost:8081/api/artikal-update/" + this.idArtikla + "?" + new URLSearchParams({
              naziv : this.trazeniArtikal.naziv,
              cena : this.trazeniArtikal.cena,
              tip: this.trazeniArtikal.tip,
              kolicina : this.trazeniArtikal.kolicina,
              opis : this.trazeniArtikal.opis,
              image : this.trazeniArtikal.photo


            }),{
            method: "PUT",
            credentials: 'include',
            headers: {
            Accept: "application/json",
            "Content-type": "application/json",
            },  
            // body: JSON.stringify(this.trazeniArtikal),
             })
            .then((response) => response.json)
            .then((data) => {
            console.log("Success : " + data);
            this.$router.push("/pregled-restorana-menadzera");
            })
            .catch((err) => {
            console.log("Error : " + err);
            alert(err);
            });
      
        },

        previewFiles(event) {
        this.trazeniArtikal.photo = event.target.files[0].name
        },


  },



}
</script>

<style scoped>

</style>

