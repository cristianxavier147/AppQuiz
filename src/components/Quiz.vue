<template>
  <section class="rebote">
    <div class="mainVerde -z-10"></div>
  </section>

  <section class="rebote">
    <div class="mainCeleste -z-10"></div>
  </section>

  <div class="border">
    <div
      v-if="data"
      class="grid grid-cols-1 gap-4 text-gray-600 mx-auto w-11/12 md:w-8/12 lg:w-10/12 overflow-y-hidden"
    >
      <div class="row-span-2">
        <div class="items-center justify-between pt-4 rounded-lg flex flex-col">
          <div
            class="p-3 w-full rounded-lg flex items-center justify-center md:p-5 mb-3"
          >
            <img
              src="../assets/interrogacion.png"
              alt="reloj"
              width="80"
              class="mr-3"
            />
            <h1
              class="text-center font-medium md:text-lg"
              v-html="data.results[currentQuestion].question"
            ></h1>
          </div>
        </div>
      </div>
      <div class="row-span-6">
        <div class="flex flex-col">
          <div class="grid grid-cols-1 gap-4 md:gap-8 md:grid-cols-2">
            <Answer
              v-for="answer in data.results[currentQuestion].shuffled_answers"
              :key="answer"
              :text="answer"
              :is-valid-answer="
                answer == data.results[currentQuestion].correct_answer
              "
              :is-invalid-answer="
                answer != data.results[currentQuestion].correct_answer
              "
              :show-answer="showAnswer"
              @check-answer="checkAnswer"
            ></Answer>
          </div>
        </div>
      </div>
      <div class="">
        <div class="min-h-full min-w-full flex items-center justify-center">
          <Transition name="grow-fade">
            <button
              @click="getNextQuestion"
              class="px-12 py-4 mb-4 bg-gray-600 text-white text-lg rounded-lg hover:bg-gray-700 transition w-full md:w-1/3"
              v-show="showAnswer"
            >
              Siguiente
            </button>
          </Transition>
        </div>
      </div>
    </div>
    <div v-else>
      <h1 class="text-center mt-12 text-gray-500">Cargando Preguntas...</h1>
    </div>
  </div>
</template>

<script>
import Answer from './Answer.vue';
import { store } from '.././store';
import { shuffle } from '.././helpers';

export default {
  components: {
    Answer,
  },
  data() {
    return {
      data: null,
      currentQuestion: 0,
      showAnswer: false,
      store,
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      fetch('https://opentdb.com/api.php?amount=5&category=23&difficulty=easy')
        .then((res) => res.json())
        .then((res) => {
          res.results.map((item) => {
            item.shuffled_answers = shuffle([
              item.correct_answer,
              ...item.incorrect_answers,
            ]);
            delete item.incorrect_answers;
          });
          this.data = res;
          this.currentQuestion = 0;
          this.showAnswer = false;
          store.setQuestionCount(res.results.length);
        });
    },
    checkAnswer(answer) {
      if (this.data.results[this.currentQuestion].correct_answer == answer) {
        this.store.incrementScore();
        this.showAnswer = true;
        this.data.results[this.currentQuestion].guessedRight = true;
        return;
      }
      this.data.results[this.currentQuestion].guessedRight = false;
      this.showAnswer = true;
    },
    getNextQuestion() {
      if (this.currentQuestion >= this.data.results.length - 1) {
        return store.endQuiz();
      }
      this.currentQuestion += 1;
      this.showAnswer = false;
    },
  },
};
</script>

<style scoped>
.grow-fade-enter-active {
  transition: all 0.2s ease-out;
}

.grow-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}

.grow-fade-enter-from,
.grow-fade-leave-to {
  opacity: 0;
  transform: scale(0.8) translateY(60px);
}

.border {
  border-radius: 50px;
  background-color: white;
  margin: 0 10em;
}

.rebote .mainVerde {
  background-color: rgb(95, 136, 95);
  border-radius: 50px;
  width: 83%;
  height: 400px;
  margin-left: 120px;
  margin-top: 15em;
  position: absolute;
  -webkit-animation-name: rebote; /* Chrome, Safari, Opera */
  -webkit-animation-duration: 2s; /* Chrome, Safari, Opera */
  -webkit-animation-iteration-count: infinite; /* Chrome, Safari, Opera */
  -webkit-animation-direction: reverse; /* Chrome, Safari, Opera */
  animation-name: rebote;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-direction: reverse;
  animation-timing-function: ease-out;
}

.rebote .mainCeleste {
  background-color: rgb(32, 182, 231);
  border-radius: 50px;
  width: 79%;
  height: 350px;
  margin-left: 150px;
  margin-top: 17em;
  position: absolute;
  -webkit-animation-name: rebote; /* Chrome, Safari, Opera */
  -webkit-animation-duration: 2s; /* Chrome, Safari, Opera */
  -webkit-animation-iteration-count: infinite; /* Chrome, Safari, Opera */
  -webkit-animation-direction: reverse; /* Chrome, Safari, Opera */
  animation-name: rebote;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-direction: reverse;
  animation-timing-function: ease-out;
}
/* Standard syntax */
@keyframes rebote {
  0% {
    left: 0px;
    top: 0px;
  }
  25% {
    left: 0px;
    top: 0px;
  }
  50% {
    left: 0px;
    top: -100px;
  }
  75% {
    left: 0px;
    top: -100px;
  }
  100% {
    left: 0px;
    top: 0px;
  }
}
</style>
