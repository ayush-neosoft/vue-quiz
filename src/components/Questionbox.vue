<template>
  <div>
    <b-jumbotron>

      <template v-slot:lead>
        <p v-html="currentQuestion.question"></p>
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(option, index) in shuffledOptions" :key="index"
          @click="selectAnswer(index)" v-html="option"
          :class="answerClass(index)"
        >
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary"
        @click="submitAnswer"
      >
        Submit
      </b-button>

      <b-button variant="success" @click.prevent="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedOption: null,
      correctOption: null,
      shuffledOptions: [],
      answered: false
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedOption = index;
    },
    shuffleOptions() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledOptions = _.shuffle(answers)
      this.correctOption = this.shuffledOptions.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer() {
      this.answered = true
      let isCorrect = false

      if(this.selectedOption === this.correctOption) {
        isCorrect = true
      }

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';

      if(!this.answered && this.selectedOption === index) {
        answerClass = 'selected'
      } else if(this.answered && this.correctOption === index) {
        answerClass = 'correct'
      } else if(this.answered && this.selectedOption === index && this.correctOption !== index) {
        answerClass = 'incorrect'
      }

      return answerClass;
    }
  },
  computed: {
    options() {
      let options = [...this.currentQuestion.incorrect_answers];
      options.push(this.currentQuestion.correct_answer);
      return options;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedOption = null;
        this.answered = false;
        this.shuffleOptions();
      }
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px;
  }
  .list-group-item:hover {
    cursor: pointer;
    background-color: #EEE
  }
  .btn {
    margin: 0 5px;
  }
  .selected {
    background-color: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: crimson;
  }
</style>