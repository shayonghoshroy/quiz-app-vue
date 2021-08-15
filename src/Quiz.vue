<!--
* Title: Quiz App
* Description: prompts the user with a series of multiple choice questions, one at a time, 
  then provides a summary of how the user answered the questions.
* Author: Shayon Ghoshroy
* Date: August 15th, 2021
* Created With: Tailwind CSS and Vue
* Designed For: Desktop and Mobile 
-->

<template>
  <!-- Body class defines font and background color -->
  <body class="font-sans text-gray-50 bg-gray-900 antialiased">
    <!-- App class defines alignment and proportions of window -->
    <div class="flex justify-center items-center w-full h-screen" id="app">
      <!-- Question container wrapper defines a limit for the width -->
      <div class="max-w-xl w-full" id="question-container-wrapper">
        <!-- Title -->
        <h1 class="text-4xl font-bold text-center text-red-400">
          {{ title }}
        </h1>
        <!-- Question Container -->
        <div
          class="bg-gray-700 p-12 rounded-lg w-full mt-3"
          id="question-container"
        >
          <!-- Display question and answers if user is going thru the quiz -->
          <div v-if="answers.length < questions.length">
            <!-- Question -->
            <p class="text-2xl font-bold text-center">
              {{ questions[index].text }}
            </p>
            <!-- Loop thru every possible answer  -->
            <label
              class="block hover:bg-green-400 cursor-pointer mt-4 border border-gray-300 rounded-lg py-2 px-6 font-bold text-lg"
              v-for="(answer, key) in questions[index].answers"
              :key="key"
              :class="{ 'bg-green-400': key == currentAnswer }"
            >
              <!-- Store chosen answer when user clicks on it -->
              <input type="radio" id="" class="hidden" @click="answers = key" />
              {{ answer }}
            </label>
          </div>
          <!-- Display results page if user is finished with quiz -->
          <div v-else>
            <!-- Defines a flex shrink grid of questions and answers -->
            <div
              v-for="(question, index) in questions"
              class="flex-shrink text-1xl font-bold text-center"
              :key="index"
            >
              {{ question.text }}
              <div class="grid grid-flow-col">
                <label
                  class="mb-7 border border-gray-300 rounded-lg"
                  v-for="(answer, key) in question.answers"
                  :key="key"
                  :class="{ 'bg-green-400': key == answers[index] }"
                >
                  {{ answer }}
                </label>
              </div>
            </div>
          </div>
          <!-- Next button only appears when a user has chosen an answer during the quiz -->
          <div class="my-7">
            <button
              class="float-right text-white font-bold px-6 py-2 bg-red-400"
              v-show="currentAnswer != null"
              @click="index = 'true'"
            >
              Next
            </button>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>


<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      // the index of the question the user is currently on
      currentIndex: 0,
      // array of ints, each int corresponds to the index of the selected answer for each question
      answerIndexes: [],
      // the current answer the user has clicked on
      currentAnswer: null,
    };
  },
  methods: {},
  computed: {
    // handle the quiz title
    title() {
      // show the current progress if a user is going thru the quiz
      if (this.answerIndexes.length < this.questions.length) {
        return this.currentIndex + 1 + "/" + this.questions.length;
        // else display a static title
      } else {
        return "Results";
      }
    },
    // handle the index of the question the user is on
    index: {
      // return the current question index
      get() {
        return this.currentIndex;
      },
      // called when the user advances to the next question
      set() {
        this.answerIndexes[this.currentIndex] = this.currentAnswer; //store the selected answer in an array
        this.currentAnswer = null; // reset current selected answer
        this.currentIndex++; // shift the current question index
      },
    },
    // handle answers
    answers: {
      // return the array of answers
      get() {
        return this.answerIndexes;
      },
      // called when a user clicks on an answer, stores the index in currentAnswer
      set(key) {
        this.currentAnswer = key;
      },
    },
  },
};
</script>
