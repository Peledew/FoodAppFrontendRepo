<template>
  <header-comp></header-comp>
  <div class="container">
    <div class="row">
      <div class="col s8">
        <h1 class="left-align">Postavljanje menadzera</h1>
        <div class="divider"></div>
        <p class="left-align fontsize1_25em">
          Ulogovani korisnik : <b>{{ ulogovaniKorisnik.korisnickoIme }}</b>
        </p>
        <p class="left-align fontsize1_25em">
          Uloga: <b>{{ ulogovaniKorisnik.uloga }}</b>
        </p>

        <h5 for="korisnickoIme">Korisničko ime menadzera:</h5>
        <input class="center-align" v-model="menadzer.korisnickoIme" />


        <a class="waves-effect waves-light btn" v-on:click="setMenager()"
          >OK</a
        >
        <br>
        <br>

        <a class="waves-effect waves-light btn" v-on:click="nazad()"
          >Nazad</a
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
  name: "PostavljanjeMenadzeraView",
  components: {
    HeaderComp,
    FooterComp,
    OpcijeComp,
  },

  data: function () {
    return {
      ulogovaniKorisnik: {},
      menadzer:{
        korisnickoIme: "",
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
    
    setMenager: function () {
      
       fetch("http://localhost:8081/api/admin/postavljanje-manadzera/"+ this.$route.query.id, {
        method: "PUT",
        credentials: 'include',
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify(this.menadzer),
      })
        .then((response) => response.json)
        .then((data) => {
          console.log("Success : " + data);
        this.$router.push("/pregled-svih-korisnika");
        })
         .catch((err) => {
              // console.log("Error : " + err);
              // alert(err);
              alert("Na raspolaganju ne postoji menadzer sa izabranim korisnickim imenom.")
            });
    },
  },


};
</script>

<style scoped></style>
