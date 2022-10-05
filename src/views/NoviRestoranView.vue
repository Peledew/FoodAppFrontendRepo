<template>
  <header-comp></header-comp>
  <div class="container">
    <div class="row">
      <div class="col s8">
        <h1 class="left-align">Kreiranje restorana</h1>
        <div class="divider"></div>
        <p class="left-align fontsize1_25em">
          Ulogovani korisnik : <b>{{ ulogovaniKorisnik.korisnickoIme }}</b>
        </p>
        <p class="left-align fontsize1_25em">
          Uloga: <b>{{ ulogovaniKorisnik.uloga }}</b>
        </p>

        <label for="nazivRestorana">Naziv restorana:</label>
        <input class="center-align" v-model="restoran.nazivRestorana" />

        <label for="tipRestorana">Tip restorana:</label>
        <input class="center-align" v-model="restoran.tipRestorana"/>

        <label for="lokacija.geografskaSirina">Geografska širina lokacije:</label>
        <input class="center-align" v-model="restoran.lokacija.geografskaSirina"/>

        <label for="lokacija.geografskaDuzina">Geografska dužina lokacije:</label>
        <input class="center-align" v-model="restoran.lokacija.geografskaDuzina"/>

         <label for="lokacija.adresa">Adresa restorana:</label>
        <input class="center-align" v-model="restoran.lokacija.adresa"/>


        <a class="waves-effect waves-light btn" v-on:click="KreirajRestoran()"
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
  name: "NoviRestoranView",
  components: {
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },

  data: function () {
    return {
      ulogovaniKorisnik: {},
      restoran:{
        nazivRestorana: "",
        tipRestorana: "",

        lokacija:{
            geografskaSirina: "",
            geografskaDuzina: "",
            adresa: "",
        }
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
    
    KreirajRestoran: function () {
      
       fetch("http://localhost:8081/api/admin/kreiranje-restorana", {
        method: "POST",
        credentials: 'include',
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify(this.restoran),
      })
        .then((response) => response.json)
        .then((data) => {
          console.log("Success : " + data);
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
