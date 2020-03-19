<template>
    <div>
        <b-jumbotron>

            <template v-slot:lead>
            {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index"
                    @click="selectedAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <b-button variant="primary" 
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button variant="success" @click="next">Next</b-button>
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
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: null,
            answered: false
        }
    },
    watch: {
        // currentQuestion(){
        //     this.selectedIndex = null
        //     this.shuffleAnswers()
        // }
        currentQuestion: {
            immediate: true,
            handler(){
                this.selectedIndex = null,
                this.answered = false,
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectedAnswer(index){
            this.selectedIndex = index
        },
        submitAnswer(){
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true

            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index){
            let answerClass = ''

            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected-answer'
            } else if(this.answered && this.correctIndex === index){
                answerClass = 'correct-answer'
            } else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                answerClass = 'incorrect-answer'
            }

            return answerClass
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

.btn{
    margin: 0 5px;
}

.list-group:hover {
    cursor: pointer;
    background: rgb(151, 97, 97);
}

.selected-answer{
    background-color: lightblue;
}

.correct-answer{
    background-color: lightgreen;
}

.incorrect-answer{
    background-color: red;
}
</style>