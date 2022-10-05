<template>
  <header-comp :enableLogin="false"></header-comp>  
    <div class="container">
        <div class="row">
          <div class="col s8">
            <h1 class="left-align">Pregled ličnih podataka</h1>
            <div class="divider"></div>
            <p class="left-align fontsize1_25em">Ulogovani korisnik : <b>{{ulogovaniKorisnik.korisnickoIme}}</b></p>
            <p class="left-align fontsize1_25em">Uloga: <b>{{ulogovaniKorisnik.uloga}}</b></p>
            

            <table class="striped">
              <tbody>
                <tr>
                  <td><b>Korisničko ime:</b></td>
                  <td>{{ulogovaniKorisnik.korisnickoIme}}</td>
                </tr>
                <tr>
                  <td><b>Ime:</b></td>
                  <td>{{ulogovaniKorisnik.ime}}</td>
                </tr>
                <tr>
                  <td><b>Prezime:</b></td>
                  <td>{{ulogovaniKorisnik.prezime}}</td>
                </tr>
                <tr>
                  <td><b>Lozinka:</b></td>
                  <td>{{ulogovaniKorisnik.lozinka}}</td>
                </tr>
                <tr>
                  <td><b>Pol:</b></td>
                  <td>{{ulogovaniKorisnik.pol}}</td>
                </tr>
                <tr>
                  <td><b>Uloga:</b></td>
                  <td>{{ulogovaniKorisnik.uloga}}</td>
                </tr>
                <tr>
                  <td><b>Datum rođenja:</b></td>
                  <td>{{formatDate(ulogovaniKorisnik.datumRodjenja)}}</td>
                </tr>
                <tr v-if="ulogovaniKorisnik.uloga === 'KUPAC' ">
                  <td><b>Tip:</b></td>
                  <td>{{ulogovaniKorisnik.tk.ime}}</td>
                </tr>
                <tr v-if="ulogovaniKorisnik.uloga === 'KUPAC' ">
                  <td><b>Bodovi:</b></td>
                  <td>{{ulogovaniKorisnik.tk.trazeniBodovi}}</td>
                </tr>
                <tr v-if="ulogovaniKorisnik.uloga === 'MENADZER' ">
                  <td><b>Restoran:</b></td>
                  <td>{{ulogovaniKorisnik.restoran.naziv}}</td>
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
    name: "pregledLicnihPodataka",
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
//  + this.$route.query.id
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
      

  },

  methods: {
    formatDate : function(date) {
            var d = new Date(date),
                month = '' + (d.getMonth() + 1),
                day = '' + d.getDate(),
                year = d.getFullYear();

            if (month.length < 2) 
                month = '0' + month;
            if (day.length < 2) 
                day = '0' + day;

            return [year, month, day].join('-');
        }
  },

}
</script>

<style scoped>
  td{
    padding-left: 5%;
  }
</style>