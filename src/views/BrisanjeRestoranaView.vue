<template>
  <header-comp></header-comp>
  <div class="container">
    <div class="row">
      <div class="col s8">
        <h1 class="left-align">Potvrda birsanja restorana</h1>
        <div class="divider"></div>
        <p class="left-align fontsize1_25em">
          Ulogovani korisnik : <b>{{ ulogovaniKorisnik.korisnickoIme }}</b>
        </p>
        <p class="left-align fontsize1_25em">
          Uloga: <b>{{ ulogovaniKorisnik.uloga }}</b>
        </p>


        <h4 text-align="left">
            Da li ste sigurni da zelite da uklonite izabrani restoran ?
        </h4>
         <hr class="solid">
        <br>

       <button class="waves-effect waves-light btn"   v-on:click="nazad()">
           Nazad
        </button>
        <br>
        <br>
        <button class="waves-effect waves-light btn"   v-on:click="deleteRestoran()">
           Ukloni
        </button>
        
      
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
  name: "BrisanjeRestoranaView",
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
    nazad: function () {
         this.$router.push("/restorani-pretraga");
    },

      deleteRestoran: function () {
      
       fetch("http://localhost:8081/api/brisanje-restorana/"+ this.$route.query.id, {
        method: "DELETE",
        credentials: 'include',
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        
      })
        .then((response) => response.json)
        .then((data) => {
          this.$router.push("/restorani");
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
