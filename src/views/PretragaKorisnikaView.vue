<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pretraga korisnika</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            
            <h6>Pretraga</h6>
            <input type="text" placeholder="Korisnicko ime..."  v-model="searchObj.korisnickoIme" />
            <input type="text" placeholder="Ime..." v-model="searchObj.ime">
            <input type="text" placeholder="Prezime..." v-model="searchObj.prezime">
            
            <a class="waves-effect waves-light btn" v-on:click="pretraga()">Pretraži</a>
           
            <br>
            <hr class="solid" id="korisnikTbl">
            <br>
            <table>
            <thead class="thead-dark">
            <tr>
                <th scope="col" style="text-align:center"><b>IME</b></th>
                <th scope="col" style="text-align:center"><b>PREZIME</b></th>
                <th scope="col" style="text-align:center"><b>KORISNICKO IME</b></th>
            </tr>
            </thead>
            <tbody>
            <tr  v-for="korisnik in korisnici" :key="korisnik.ime">
                <td style="text-align:center">{{korisnik.ime}}</td>
                <td style="text-align:center">{{korisnik.prezime}}</td>
                <td style="text-align:center">{{korisnik.korisnickoIme}}</td>
            </tr>
            </tbody>
        </table>

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
    name: "PretragaKorisnikaView",
    components:{
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },
    data: function(){
      return{
        ulogovaniKorisnik:{
            korisnik:{},
        },
        searchObj:{
            korisnickoIme:"",
            ime:"",
            prezime:"", 
           
        },
        korisnici: [],
        
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
    
    pretraga: function () {
      
       fetch("http://localhost:8081/api/admin-pregled/pretraga", {
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
              this.korisnici = data
                  var x = document.getElementById("korisnikTbl"); 
                  x.style.visibility = "visible";
            })
            .catch((err) => {
              // console.log("Error : " + err);
              // alert(err);
              alert("Korisnik nije pronađen. Proverite da li ste uneli odgovarajuće parametre pretrage")
              var x = document.getElementById("korisnikTbl"); 
                  x.style.visibility = "hidden";
            });
    },
  },
}
</script>

<style scoped>
</style>