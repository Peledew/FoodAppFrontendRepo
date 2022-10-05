<template>
    <header-comp></header-comp>
    <div class="row">
        <div class="homepage_header relativePosition noRepeatBackground">
          <div class="pos_absolte header_natpis pading5 blur">
            <h3 class="left-align">Postoji li nešto bolje od toga da se <br> hrana sama spremi i dođe do Vas?</h3>
            <p class="fontsize2vw">~ Naruči i uživaj ~</p>
          </div>
        </div>
    </div>

    <div class="container">
      <div class="row margin12_0">
        <h1 class="fontfamiliCherrySwash">Hrana po vašem ukusu</h1>
        <div class="col s3">
          <div class="card">
            <div class="card-image z-depth-3">
              <img src="../assets/pice.jpg" alt="slika_pica">
              <span class="card-title fontsize2vw">Pice</span>
            </div>
          </div>
        </div>

        <div class="col s3">
          <div class="card">
            <div class="card-image z-depth-3">
              <img src="../assets/burgeri.jpg" alt="slika_burgera">
              <span class="card-title fontsize2vw">Burgeri</span>
            </div>
          </div>
        </div>

        <div class="col s3">
          <div class="card">
            <div class="card-image z-depth-3">
              <img src="../assets/slatkisi.jpg" alt="slika-slatkisa">
              <span class="card-title fontsize2vw">Slatkiši</span>
            </div>
          </div>
        </div>

        <div class="col s3">
          <div class="card">
            <div class="card-image z-depth-3">
              <img src="../assets/sokovi.jpg" alt="slika-sokova">
              <span class="card-title fontsize2vw">Piće</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
          <div class="kuvarSection"></div>
      </div>
  <footer-comp></footer-comp>
</template>

<script>
import FooterComp from '../components/FooterComp.vue';
import HeaderComp from '../components/HeaderComp.vue';
export default {
  name: "HomeView",
  components:{
    HeaderComp,
    FooterComp
  },


};
</script>

<style>
.homepage_header {
   background-image: url("../assets/header_back.jpg"); 
   height: 770px; 
   max-width: 100%;
   background-size: 100% 100%;
}

.kuvarSection {
   background-image: url("../assets/kuvar.png"); 
   height: 560px; 
   max-width: 100%;
   background-size: 100% 100%;
}

.noRepeatBackground{
     background-repeat: no-repeat;
}

.relativePosition{
  position: relative;
}

.nomargin{
  margin: none;
}

 @media screen and (max-width:600px) {
    .homepage_header{
      background-size: 100% 50%;
    }

  .header_natpis{
    top:12%;
    left: 3%;
  }
}

</style>