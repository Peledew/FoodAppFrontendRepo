<template>
  <div class="col s6">
      <div class="card medium">
        <div class="card-image waves-effect waves-block waves-light">
          <img v-if = "korisnik.pol === 'M' " class="activator card_image" src="@/assets/male_avatar.jpg" alt="slika_profila">
          <img v-else class="activator card_image" src="@/assets/female_avatar.jpg" alt="slika_profila">
        </div>

        <div class="card-content">
          <span class="card-title activator grey-text text-darken-4 margin_left_9">{{korisnik.korisnickoIme}}<i class="material-icons right">more_vert</i></span>
          <p><i>{{korisnik.uloga}}</i></p>
        </div>

        <div class="card-reveal">
          <span class="card-title grey-text text-darken-4 margin_left_9">Informacije<i class="material-icons right">close</i></span>
          <table class="striped">
                <tbody>
                  <tr>
                    <td><b>Korisničko ime:</b></td>
                    <td>{{korisnik.korisnickoIme}}</td>
                  </tr>
                  <tr>
                    <td><b>Ime:</b></td>
                    <td>{{korisnik.ime}}</td>
                  </tr>
                  <tr>
                    <td><b>Prezime:</b></td>
                    <td>{{korisnik.prezime}}</td>
                  </tr>
                  <tr>
                    <td><b>Lozinka:</b></td>
                    <td>{{korisnik.lozinka}}</td>
                  </tr>
                  <tr>
                    <td><b>Pol:</b></td>
                    <td>{{korisnik.pol}}</td>
                  </tr>
                  <tr>
                    <td><b>Uloga:</b></td>
                    <td>{{korisnik.uloga}}</td>
                  </tr>
                  <tr>
                    <td><b>Datum rođenja:</b></td>
                    <td>{{formatDate(korisnik.datumRodjenja)}}</td>
                  </tr>
                  <tr v-if = "korisnik.uloga === 'MENADZER'">
                    <td><b>Restoran:</b></td>

                    <td v-if="korisnik.restoran !== null">{{korisnik.restoran.naziv}}</td>

                  </tr>
                  <tr v-if = "korisnik.uloga === 'KUPAC'">
                    <td><b>Tip kupca:</b></td>
                    <td>{{korisnik.tk.ime}}</td>
                  </tr>
                  <tr v-if = "korisnik.uloga === 'KUPAC'">
                    <td><b>Broj sakupljenih bodova:</b></td>
                    <td>{{korisnik.brojSakupljenihBodova}}</td>
                  </tr>
                </tbody>
              </table>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "KorisnikCardKomponenta",
  props: ["korisnik"],
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
};
</script>


<style>
</style>
