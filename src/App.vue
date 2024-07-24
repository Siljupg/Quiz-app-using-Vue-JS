<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    question: 'What is Vue JS?',
    answer: 0,
    options: [
      "A front end Framework",
      "A Library",
      "A icecream maker"
    ],
    selected: null
  },
  {
    question: 'What is Vuex?',
    answer: 2,
    options: [
      "Vue with an X",
      "A Cheese selection",
      "A state management library"
    ],
    selected: null
  },
  {
    question: 'What is Vue Router used for?',
    answer: 1,
    options: [
      "Walking in space",
      "A Routing Library for Vue JS",
      "Inbuilt Function"
    ],
    selected: null
  }
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.forEach(q => {
    if (q.selected === q.answer) {
      value++;
    }
  });
  return value;
});

const getcurrentQuestion = computed(() => {
  return questions.value[currentQuestion.value];
});

const setAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = parseInt(evt.target.value, 10);
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getcurrentQuestion.question }}</span>
        <span class="score">Score {{ score }} / {{ questions.length }}</span>
      </div>
      <div class="options">
        <label v-for="(option, index) in getcurrentQuestion.options" 
               :key="index"
               :class="`option ${
                 getcurrentQuestion.selected === index
                   ? index === getcurrentQuestion.answer
                     ? 'correct'
                     : 'wrong'
                   : ''
               } ${
                 getcurrentQuestion.selected != null &&
                 index !== getcurrentQuestion.selected
                   ? 'disabled'
                   : ''
               }`">
          <input 
            type="radio" 
            :name="getcurrentQuestion.question"
            :value="index"
            v-model="getcurrentQuestion.selected"
            :disabled="getcurrentQuestion.selected !== null"
            @change="setAnswer">
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="nextQuestion" 
              :disabled="getcurrentQuestion.selected === null">
        {{ 
          currentQuestion.value === questions.length - 1
            ? 'Finish'
            : 'Next Question'
        }}
      </button>
    </section>
    <section v-else>
      <h2>You have finished the Quiz!</h2>
      <p>Your Score is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: #2cce7d;
}

.option.wrong {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button.disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}
</style>
