<template>
  <div class="container">
    <img :src="fuente" alt="no se puede mostrar" />

    <p>{{ texto }}</p>
  </div>
</template>

<script>
export default {
  name: "Juego",

  props: {
    texto: {
      type: String,
      required: true,
    },

    fuente: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      img: this.fuente,
    };
  },
};
</script>

<style>


.container {
  display: flex;
  flex-direction: column;
}
img{
    margin: 0px 15px;
    width: 350px;
    height: 400px;
}

p{
    font-size: 25px;
}

</style>