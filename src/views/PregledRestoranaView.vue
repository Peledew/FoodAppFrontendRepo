<template>
  <header-comp></header-comp>
  <div class="container">
    <div class="row">
      <div class="col s8">
        <p class="left-align fontsize1_25em">
          Ulogovani korisnik : <b>{{ ulogovaniKorisnik.korisnickoIme }}</b>
        </p>
        <p class="left-align fontsize1_25em">
          Uloga: <b>{{ ulogovaniKorisnik.uloga }}</b>
        </p>
        <div class="divider"></div>
        <h1>{{ restoran.naziv }}</h1>
        <div class="divider"></div>
  
        <p class="left-align fontsize1_25em">Tip restorana : {{restoran.tipRestorana}}</p>
        <p class="left-align fontsize1_25em">Status : {{restoran.status}}</p>
        <p class="left-align fontsize1_25em">Ocena : {{restoran.prosecnaOcena}}</p>
        
        <div class="divider"></div>
        <h5 style="text-align:left">Cenovnik</h5>
        <br>
        <table class="table">
        <thead class="thead-dark">
          <tr>
            <th scope="col" style="text-align:center"><b>NAZIV</b></th>
            <th scope="col" style="text-align:center"><b>RSD</b></th>
            <th scope="col" style="text-align:center"><b>OPIS</b></th>
          </tr>
        </thead>
        <tbody>
          <tr  v-for="artikal in restoran.artikli" :key="artikal.naziv">
            <td style="text-align:center">{{artikal.naziv}}</td>
            <td style="text-align:center">{{artikal.cena}}</td>
            <td style="text-align:center">{{artikal.opis}}</td>
          </tr>
        </tbody>
      </table>
  
      <br>
        <h5 style="text-align:left">Komentari</h5>
      <br>
      <div class="divider"></div>
      <ul>
        <li  v-for="kom in restoran.komentari" :key="kom.korisnickoIme">

          <div class="left-align fontsize1_25em">
            <b>korisnicko ime: </b> <i>{{kom.korisnickoIme}}</i></div>

            <div class="left-align fontsize1_25em">
            <b>Ocena: </b>{{kom.ocena}}</div>


            <div class="comments">
            {{kom.textKomentara}}</div>
            <br>
        </li>
      </ul>

      

      </div>
      <div class="col s4">
        <opcije-comp :uloga="ulogovaniKorisnik.uloga"></opcije-comp>
      </div>
    </div>
  </div>
  <footer-comp></footer-comp>
</template>

<script>
import FooterComp from "../components/FooterComp.vue";
import HeaderComp from "../components/HeaderComp.vue";
import OpcijeComp from "../components/OpcijeComp.vue";
export default {
  name: "PregledRestoranaView",
  components: {
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },

  data: function () {
    return {
      ulogovaniKorisnik: {},
       restoran:{},
    };
  },
  //  + this.$route.query.id
  mounted: function () {
    fetch("http://localhost:8081/api/pregled-licni-podaci/", {
      credentials: "include",
    })
      .then((response) => response.json())
      .then((data) => {
        // console.log("Restoran : ", data.restoran.naziv)
        console.log("Success:", data);
        this.ulogovaniKorisnik = data;
      })
      .catch((error) => {
        console.error("Error:", error);
      });

        fetch('http://localhost:8081/api/pretraga/'+ this.$route.query.id, {
        credentials: "include",
        })
        .then(response => response.json())
        .then(data => {
          console.log("Success:", data); this.restoran = data
        
        })
        .catch((error) => {
          console.error("Error:", error);
        });
  },

  methods: {
    
    
  },


};
</script>

<style scoped></style>
