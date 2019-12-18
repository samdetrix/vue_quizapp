<template>
    <div>
        <b-jumbotron>
           
            <template v-slot:lead>
                
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click="answer_submit(index)"
                    :class="answeredClass(index)"
                    
                    
                  
                    >
                    {{ answer }}
                </b-list-group-item>               
            </b-list-group>
           

            <b-button 
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered"
            variant="primary"            
            >Submit</b-button>
            <b-button @click.prevent="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
</div>

</template>
<script lang="ts">
import Vue from 'vue'
import _ from 'lodash'

export default Vue.extend({
    props: {
        currentQuestion: Object,
        next: Function,
        increament: Function
        
    },
    data(){
        return{
            selectedIndex: null,
            shuffledAnswers: [],
            answered: false,
            correctIndex: null
        }
    },
    computed:{
        answers(){
            let answers =[...this.currentQuestion.incorrect_answers]
           answers.push(this.currentQuestion.correct_answer) 
            return answers

        }
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex=null
                this.shuffleAnswers()
                this.answered=false
            }
        }
    },

    methods:
    {
        answer_submit(index){
            this.selectedIndex =index
            
        },
        submitAnswer(){
            let isCorrect = false
            if(this.selectedIndex===this.correctIndex){
                isCorrect =true
            }
            this.answered = true
            this.increament(isCorrect)
            
        },
        shuffleAnswers(){
                let answers =[...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        
        answeredClass(index){
            let answeredClass = ''
            if (!this.answered && this.selectedIndex === index) {
                answeredClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answeredClass = 'correct'
            } else if (this.answered && this.selectedIndex===index && this.correctIndex !== index) {
                answeredClass = 'incorrect'
            } 
                return answeredClass
            
        }
        
    },
    
    
    
})
</script>
 
 <style  scoped>
    .list-group{
        margin:10px;
    }

    .btn{
        margin: 0 10px;
    }
    .list-group-item:hover{
        background: lightgray;
        cursor: pointer;
    }
    .selected{
        background-color: rgb(156, 172, 179);
    }
    .correct{
        background-color: skyblue;
    }
    .incorrect{
        background-color:lightcoral;
    }
 </style>