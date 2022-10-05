<template>
  <header-comp></header-comp>
  <div class="container">
    <div class="row">
      <div class="col s8">
        <h1 class="left-align">Dodaj novog dostavljača</h1>
        <div class="divider"></div>
        <p class="left-align fontsize1_25em">
          Ulogovani korisnik : <b>{{ ulogovaniKorisnik.korisnickoIme }}</b>
        </p>
        <p class="left-align fontsize1_25em">
          Uloga: <b>{{ ulogovaniKorisnik.uloga }}</b>
        </p>

        <label for="korisnickoIme">Korisničko ime:</label>
        <input class="center-align" v-model="dostavljac.korisnickoIme" />

        <label for="ime">Ime:</label>
        <input class="center-align" v-model="dostavljac.ime"/>

        <label for="prezime">Prezime:</label>
        <input class="center-align" v-model="dostavljac.prezime"/>

        <label for="lozinka">Lozinka:</label>
        <input class="center-align" v-model="dostavljac.lozinka"/>

         <label for="pol">Pol:</label>
        <input class="center-align" v-model="dostavljac.pol"/>


        <a class="waves-effect waves-light btn" v-on:click="dodajDostavljaca()"
          >Kreiraj</a
        >
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
  name: "NoviDostavljacView",
  components: {
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },

  data: function () {
    return {
      ulogovaniKorisnik: {},
      dostavljac:{
        ime: "",
        prezime: "",
        korisnickoIme: "",
        lozinka: "",
        uloga : "DOSTAVLJAC",
        pol : "M",
      }
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
  },

  methods: {
    
    dodajDostavljaca: function () {
      
       fetch("http://localhost:8081/api/admin/dodavanje-dostavljaca", {
        method: "POST",
        credentials: 'include',
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify(this.dostavljac),
      })
        .then((response) => response.json)
        .then((data) => {
          console.log("Success : " + data);
          this.$router.push("/pregled-svih-korisnika");
        })
        .catch((err) => {
          console.log("Error : " + err);
          alert(err);
        });
    },
  },


};
</script>

<style scoped></style>
