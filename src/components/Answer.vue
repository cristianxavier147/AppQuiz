<template>
  <button
    class="w-full bg-blue-800 text-white rounded-3xl p-4 transition md:text-lg md:p-6"
    @click="$emit('checkAnswer', text)"
    v-html="text"
    :disabled="showAnswer"
    :class="{
      'bg-red-900': showAnswer && isInvalidAnswer,
      'bg-red-600': showAnswer && isValidAnswer,
      'hover:bg-blue-500': !showAnswer,
    }"
  ></button>
</template>

<script>
export default {
  props: {
    text: String,
    isValidAnswer: Boolean,
    isInvalidAnswer: Boolean,
    showAnswer: Boolean,
  },
  data() {
    return {};
  },
  methods: {},
};
</script>
