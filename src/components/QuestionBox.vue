<template>
  <div class="question-box-container">
    <b-jumbotron >
      

    <template #lead>
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">


    <b-list-group>
      <b-list-group-item v-for="(answer, index) in answers"
       :key="index"
       @click="selectAnswer(index)"
       :class="[
       !answered && selectedIndex === index ? 'selected' : 
        answered && correctIndex === index ? 'correct' : ''
        ]"
       >
        {{ answer }}
        </b-list-group-item>
    </b-list-group>
    

    <b-button 
    variant="primary" 
    v-on:click="submitAnswer"
    :disabled="selectedIndex === null || answered"
    >Submit</b-button>
    <b-button @click="next" variant="success">
      Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data: function(){
    return{
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  watch: {
    currentQuestion:{
      immediate: true,
      handler(){
        this.answered = false
        this.selectedIndex = null
        this.shuffleAnswers()
      }
    }
    
  /*  (){
      this.selectedIndex = null
      this.shuffleAnswers()
    }*/
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex = index
      console.log(index)
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      console.log(this.correctIndex)
    },
    submitAnswer(){
      let isCorrect = false
      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }

      this.answered = true
      this.increment(isCorrect)
    }
  },
  computed: {
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  }
}
</script>

<style scoped>
.list-group{
  margin-bottom: 15px;
  }

.list-group-item:hover{
  background: #EEE;
  cursor: pointer;
  }
  .btn{
    margin: 0 5px;
  }

  .selected{
    background-color: lightblue;
  }
  .correct{
    background-color: green;
  }
  .incorrect{
    background-color: red;
  }
</style>