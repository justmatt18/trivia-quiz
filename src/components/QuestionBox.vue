<template>
    <div class="my-5">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentTrivia.question }}
            </template>

            <hr class="my-4">

            <div class="answer-list">
                <b-list-group>
                    <b-list-group-item
                        v-for="(answer, index) in shuffledAnswers" :key="index"
                        @click.prevent="selectedAnswer(index)"
                        :class="answerClass(index)"
                    >
                        {{ answer }}
                    </b-list-group-item>
                </b-list-group>
            </div>

            <b-button 
                v-show="!testAgain" 
                class="btn" variant="primary" 
                @click.prevent="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button 
                v-show="!testAgain" 
                @click="next" 
                class="btn" variant="success" 
                :disabled="index+1 === 10">
                Next
            </b-button>
            <b-button block v-show="testAgain" @click="resetTestAgain" variant="primary">Test Again</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import lodash from 'lodash'
export default {
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: null,
            summary: '',
            testAgain: false,
        }
    },
    props: {
        currentTrivia: Object,
        index: Number,
        next: Function,
        increment: Function,
        getTrivias: Function,  
    },
    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [...this.currentTrivia.incorrect_answers, this.currentTrivia.correct_answer]
            this.shuffledAnswers = lodash.shuffle(answers)
        },
        submitAnswer() {
            let isCorrect = false;
            this.answered = true;

            if (this.shuffledAnswers[this.selectedIndex] === this.currentTrivia.correct_answer) {
                isCorrect = true
            }
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentTrivia.correct_answer)

            this.hasSubmittedLastItem()
            return this.increment(isCorrect)
        },
        hasSubmittedLastItem() {
            let itemNo = this.index + 1
            if (itemNo == 10 && this.answered == true) {
                this.testAgain = true   
            }
            return this.testAgain;
        },
        resetTestAgain() {
            this.getTrivias()
            this.testAgain = false
        },
        answerClass(index) {
            let answerClass = ''
            if (!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'incorrect'
            }

            return answerClass
        },
        decodeHtml(html) {
            var question = document.createElement("textarea");
            question.innerHTML = html;
            return question.value;
        }
    },
    watch: {
        currentTrivia: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = null
                this.shuffleAnswers()
            }
        },
        
    },

}
</script>

<style scoped>
    .btn {
        margin: 0 5px;
    }
    
    .list-group {
        margin-bottom: 15px;
    }
        
    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }

    .selected {
        background-color: lightblue;
    }
    
    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: red;
    }
        
    
</style>