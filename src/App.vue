<template>

<div class="contenedor">
 <div class="juego" v-if="juego">
    <h2>Puntaje: {{ puntaje }}</h2>
    <h2>Intentos: {{ intento }}</h2>
    <div class="imagenes">
      <Juego :texto="cadena1" :fuente="dir1" />
      <Juego :texto="cadena2" :fuente="dir2" />
      <Juego :texto="cadena3" :fuente="dir3" />
    </div>

    <button @click="jugar()">JUGAR</button>
  </div>

  <div class="mensajelose" v-if="this.win == false">
    <h2>Has utilizado tus 5 intentos</h2>
    <h2>El juego ha terminado, intentalo nuevamente</h2>
    <button @click="reset()">Nuevo Juego</button>
  </div>

  <div class="mensajewin" v-if="this.win == true">
    <h2>Puntaje: {{ this.puntaje }}</h2>
    <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
    <button @click="reset()">Nuevo Juego</button>
  </div>

</div>
 
</template>

<script>
import Juego from "./components/Juego.vue";

export default {
  name: "App",
  components: {
    Juego,
  },

  data() {
    return {
      cadena1: "XXXXXXXXXXXXXXXXXXX",
      cadena2: "XXXXXXXXXXXXXXXXXXX",
      cadena3: "XXXXXXXXXXXXXXXXXXX",

      dir1: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
      dir2: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
      dir3: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",

      puntaje: 0,
      intento: 0,
      win: null,
      juego:true,
    };
  },

  methods: {
    async jugar() {
      const data1 = await fetch("https://yesno.wtf/api").then((r) => r.json());
      this.cadena1 = data1.answer;
      this.dir1 = data1.image;

      const data2 = await fetch("https://yesno.wtf/api").then((r) => r.json());
      this.cadena2 = data2.answer;
      this.dir2 = data2.image;

      const data3 = await fetch("https://yesno.wtf/api").then((r) => r.json());
      this.cadena3 = data3.answer;
      this.dir3 = data3.image;

      this.intento += 1;

      if (
        this.cadena1 == "yes" &&
        this.cadena2 == "yes" &&
        this.cadena3 == "yes"
      ) {
        this.puntaje += 5;
      } else if (
        (this.cadena1 == "yes" && this.cadena2 == "yes") ||
        (this.cadena2 == "yes" && this.cadena3 == "yes") ||
        (this.cadena1 == "yes" && this.cadena3 == "yes")
      ) {
        this.puntaje += 2;
      } else if (
        this.cadena1 == "yes" ||
        this.cadena2 == "yes" ||
        this.cadena3 == "yes"
      ) {
        this.puntaje += 1;
      } else {
        this.puntaje += 0;
      }

      if (this.intento == 5 && this.puntaje < 10) {
        this.win = false;
        this.juego=false;
      } else if (this.puntaje >= 10) {
        this.win = true;
        this.juego=false;
      }
    },

    reset() {
      this.cadena1 = "XXXXXXXXXXXXXXXXXXX";
      this.cadena2 = "XXXXXXXXXXXXXXXXXXX";
      this.cadena3 = "XXXXXXXXXXXXXXXXXXX";

      this.dir1 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg";
      this.dir2 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg";
      this.dir3 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg";

      this.puntaje = 0;
      this.intento = 0;
      this.win = null;
      this.juego=true;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.imagenes{
    display: flex;
    flex-direction: row;
}
.mensajelose{
  color: red;
}

.mensajewin{
  color: blue;
}

button{

  width: 120px;
  height: 100px;

  font-size: 25px;
}

.contenedor{

 display: flex;
  align-items: center;
  justify-content: center;
 
}
</style>