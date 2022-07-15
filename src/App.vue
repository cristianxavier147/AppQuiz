<template>
  <div class="h-2 bg-blue-500"></div>
  <div class="h-5 w-4/12 bg-yellow-500"></div>
  <div class="imagenes">
    <img
      src="./assets/reloj-de-arena.png"
      alt="reloj"
      width="120"
      class="mt-5"
    />
    <img src="./assets/cronometro.png" alt="reloj" width="120" class="mt-5" />
  </div>

  <div v-if="store.quizEnded">
    <Restart></Restart>
  </div>
  <div v-else>
    <Quiz></Quiz>
  </div>
</template>

<script>
import Quiz from './components/Quiz.vue';
import Restart from './components/Restart.vue';
import { store } from './store';

export default {
  components: {
    Quiz,
    Restart,
  },
  data() {
    return {
      score: 0,
      store,
    };
  },
  created() {},
  methods: {},
};
</script>

<style scoped>
body {
  font-family: 'Epilogue';
  background-image: url(./assets/fondo.webp);
}
input,
textarea,
button,
select,
a {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

.imagenes {
  display: flex;
  justify-content: center;
  margin: 10px auto;
}
</style>
