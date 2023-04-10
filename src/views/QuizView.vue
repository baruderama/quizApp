<script setup>
    import Question from "../components/Question.vue"
    import QuizHeader from "../components/QuizHeader.vue"
    import {useRoute} from "vue-router"
    import {ref, watch,computed} from "vue"
    import quizes from "../data/quizes.json"
    import Result from "../components/Result.vue"

    const route = useRoute()

    const quizId = parseInt(route.params.id)

    const quiz = quizes.find(q=> q.id === quizId)

    const currentQuestionIndex = ref(0)
    const numberOfCorrectAnswers = ref(0)
    const showResult =ref(false)

    // const questionStatus= ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

    // //esto hace que cuando cambie el currentQuestionIndex se desate una funcion que en este caso
    // //es volver a hacer la operacion de arrriba
    // watch (() => currentQuestionIndex.value, ()=> {
    //     questionStatus.value= `${currentQuestionIndex.value}/${quiz.questions.length}`
    // }) 

    //forma mas corta con computed ya que computed esta miradno siempre el estado de una 
    //variable y al mismo tiempo lo referencia
    const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
    const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length *100}%`)
    const onOptionSelected =(isCorrect) => {
        console.log("EMITED EVENT",isCorrect)
        if(isCorrect){
            numberOfCorrectAnswers.value++
        }

        if(quiz.questions.length-1 === currentQuestionIndex.value){
            showResult.value =true
        }

        currentQuestionIndex.value++
    }

</script>


<template>
    
    <div>
        
        <QuizHeader 
        :questionStatus="questionStatus"
        :barPercentage="barPercentage"
        />
        <div >
            <Question 
            v-if="!showResult"
            :question="quiz.questions[currentQuestionIndex]" 
            @selectOption="onOptionSelected"
            />
            <Result  
            v-else
            :quizQuestionLength="quiz.questions.length"
            :numberOfCorrectAnswers="numberOfCorrectAnswers"
            
            />
        </div>
        {{ numberOfCorrectAnswers }}
        <button @click="currentQuestionIndex++">Next Question</button>

       
        
    </div>
</template>

<style scoped>
    

    
</style>