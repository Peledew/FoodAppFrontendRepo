<template>
      <table class="striped z-depth-1">
        <tbody>
          <tr>
            <td><b>UUID:</b></td>
            <td>{{porudzbina.uuid}}</td>
          </tr>
          <tr>
            <td><b>Stavke porudžbine:</b></td>
            <td>
                <ul class = "list_style_type_none">
                    <li  v-for = "stavka in porudzbina.stavkePorudzbine" :key ="stavka.artikal.naziv">{{stavka.porucenaKolicina}}x, {{stavka.artikal.naziv}}, <i>{{stavka.artikal.cena}}din.</i> </li>
                </ul>
            </td>
          </tr>
          <tr v-if="restoran !== null ">
            <td><b>Restoran:</b></td>
            <td>{{restoran.naziv}}</td>
          </tr>
          <tr>
            <td><b>Datum i vreme porudzbine:</b></td>
            <td>{{porudzbina.datumIVreme}}</td>
          </tr>
          <tr>
            <td><b>Cena:</b></td>
            <td>{{porudzbina.cena}}</td>
          </tr>
          <tr v-if="kupac !== null ">
            <td><b>Kupac:</b></td>
            <td>{{kupac.korisnickoIme}}</td>
          </tr>
          <tr>
            <td><b>Status:</b></td>
            <td>{{porudzbina.status}}</td>
          </tr>
          <tr v-if="kupac !== null ">
            <td><b>Ostavi komentar:</b></td>
            <td v-if="porudzbina.status === 'dostavljena' ">
              <button id = "batn" class="waves-effect waves-light btn"  v-on:click="ostaviKomentar()">Komentariši</button>
            </td>
          </tr>
        </tbody>
      </table>
</template>

<script>
export default {
  name: "PorudzbinaComp",
  props: ["porudzbina", "kupac", "restoran"],
  methods: {
    ostaviKomentar: function () {
      document.cookie = "uuidporudzbine=" + this.porudzbina.uuid;
      this.$router.push("/ostavi-komentar");
    },
  },
};
</script>


<style scoped>
table{
    margin-bottom: 10%;
}
</style>
