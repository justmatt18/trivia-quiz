<template>
  <div id="app">
    <b-container fluid>
      <Header 
        :numCorrect = "numCorrect"
        :totalAnswered =" totalAnswered"
        :itemNo = "index"
      />
      <b-container>
        <b-row>
          <b-col md="6" offset="md-3" sm="12">
            <QuestionBox 
              v-if="trivias.length"
              :currentTrivia = "trivias[index]"
              :index="index"
              :next = "next"
              :increment = "increment"
              :getTrivias = "getTrivias"
            />
          </b-col>
        </b-row>
      </b-container>
    </b-container>
    
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox'
import { async } from 'q'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      btnTitle: 'Get Trivias',
      trivias: [],
      index: 0,
      numCorrect: 0,
      totalAnswered: 0,
      testAgain: false,
    }
  },
  created() {
    this.getTrivias()
  },
  methods: {
    async getTrivias() {
      this.index = 0
      this.numCorrect = 0
      this.totalAnswered = 0

      const res = await fetch('https://opentdb.com/api.php?amount=10&category=18&difficulty=medium&type=multiple', {
        method: 'GET'
      })
      const data = await res.json()
      return this.trivias = data.results
    },
    next() {
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      }
      this.totalAnswered++
    }
  }
}
</script>

<style lang="stylus">
  #app
    text-align: center;
</style>