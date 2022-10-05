<template>
  <header-comp :enableLogin="false"></header-comp> 
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Komentar</h1>

            <div class="row">
                <div class="input-field col s12">
                    <textarea id="textarea1" class="materialize-textarea" v-model="noviKomentar.tekstKomentara"></textarea>
                    <label for="textarea1">Komentar...</label>
                </div>
            </div>

            <div class="row">
                <div class="col s12">
                    <label for="kolicina">Ocena:</label>
                    <input class = "center-align" v-model="noviKomentar.ocena" />
                </div>
            </div>
            

            <a class="waves-effect waves-light btn"  v-on:click="dodajKomentar()">Ostavi komentar</a>
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
        noviKomentar: {},
        uuid_porudzbine:{},
      };
    },

    mounted: function () {

    let x = document.cookie
    this.uuid_porudzbine = x.substring(15)


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
        dodajKomentar : function(){
            fetch("http://localhost:8081/api/kupac-pregled-porudzbina/" + this.uuid_porudzbine,{
            method: "POST",
            credentials: 'include',
            headers: {
            Accept: "application/json",
            "Content-type": "application/json",
            },  
            body: JSON.stringify(this.noviKomentar),
             })
            .then((response) => response.json)
            .then((data) => {
            console.log("Novi komentar : " + data);
            document.cookie = "uuidporudzbine=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
            this.$router.push("/pregled-porudzbina-kupca");
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

</style>

