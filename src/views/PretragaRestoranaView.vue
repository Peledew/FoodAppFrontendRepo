<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pronađi restoran</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            <h6>Pretraga</h6>
            <input type="text" placeholder="Naziv..."  v-model="searchObj.naziv" />
            <input type="text" placeholder="Tip restorana..." v-model="searchObj.tipRestorana">
            <input type="text" placeholder="Adersa..." v-model="searchObj.adresaLokacije">
            
            <a class="waves-effect waves-light btn" v-on:click="pretraga()">Pretraži</a>
            
            <!--Za rezultat pretrage-->
            <ul class="collapsible visibility_hidden" id="restoran_kolapsibl">
                <li v-for="r in restorani" :key="r.naziv">
                    <div class="collapsible-header"><i class="material-icons">restaurant</i>{{r.naziv}}
                    

                      <button class="delbtnRestoran"  v-on:click="ukloniRestoran(r)" v-if = "ulogovaniKorisnik.uloga === 'ADMIN' " >
                        Ukloni restoran
                      </button>

                       <button class="setMenager"  v-on:click="postaviMenadzera(r)" v-if = "ulogovaniKorisnik.uloga === 'ADMIN' " >
                       Postavi <br>menadzera
                      </button>
                     
                       <button class="btnSeeMore"  v-on:click="seeMore(r)" >
                      Više o restoranu
                      </button>
                    
                    </div>
                    
                     
                    <div class="collapsible-body">
                        <span><i class="material-icons">location_on</i>{{r.lokacija.adresa}}</span><br>
                        <span><i class="material-icons">restaurant</i>{{r.tipRestorana}}</span><br>
                        <span><i class="material-icons">restaurant</i>{{r.status}}</span><br>
                    </div>
                </li>
            </ul>
           
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
        searchObj:{
            naziv:"",
            tipRestorana:"",
            adresaLokacije:"", 
            statusRestorana: null,  //nema pretrage po statusu ali je prosiren dto
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
      
  },
   methods: {

     seeMore: function (restoran) {
      this.$router.push("/restoran?id=" + restoran.id);
    },

     ukloniRestoran: function (restoran) {
      this.$router.push("/deleteRestoran?id=" + restoran.id);
    },

      postaviMenadzera: function (restoran) {
      this.$router.push("/postavi-menadzera?id=" + restoran.id);
    },
    
    pretraga: function () {
      
       fetch("http://localhost:8081/api/pretraga", {
            method: "POST",
            credentials: 'include',
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
            },
            
            body: JSON.stringify(this.searchObj),
            })
            .then((response) => response.json())
            .then((data) => {
              console.log("Success : " + data);
              this.restorani = data

                  var x = document.getElementById("restoran_kolapsibl"); 
                  x.style.visibility = "visible";

            })
            .catch((err) => {
              // console.log("Error : " + err);
              // alert(err);
              alert("Restoran nije pronađen. Proverite da li ste uneli odgovarajuće parametre pretrage")
              var x = document.getElementById("restoran_kolapsibl"); 
                  x.style.visibility = "hidden";
            });
    },


  },
}
</script>

<style scoped>
</style>