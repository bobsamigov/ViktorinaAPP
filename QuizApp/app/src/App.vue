<script setup>

import {ref,computed} from 'vue'

const currentQuestion=ref(0)
const isCompleted = ref(false)
function back(){
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value--
  }
}

const GetCurrentQuestion= computed(()=>{
  let question= questions.value[currentQuestion.value]
  question.index=currentQuestion.value
  return question
})
const setAnswer=(e)=>{
  questions.value[currentQuestion.value].selected=e.target.value}
  


const nextQuestion=()=>{
if(currentQuestion.value<questions.value.length-1){
  currentQuestion.value++
}
else {
  isCompleted.value=true
}
}

const score=computed(()=>{
  let value=0
  questions.value.map((question) => {
    if(question.selected==question.answer){
      value++
    }
  })
  return value
})





const questions =ref([
  {
    id:1,
    title:'What was meta platform?',
    options:['Facebook', 'Amazon','Yandex'],
    answer:0,
    selected:null
  },
  {
    id: 2,
    title: 'What was tiger family',
    options: ['dog', 'mouse', 'cat'],
    answer: 2,
    selected: null
  },
  {
    id: 3,
    title: 'O\'zbekiston poytaxti?',
    options: ['Bishkek', 'Tashkent', 'Alama ata'],
    answer: 1,
    selected: null
  },
  {
    id: 4,
    title: 'Angliya poytaxti?',
    options: ['Bishkek', 'Tashkent', 'London'],
    answer: 2,
    selected: null
  }
])

</script>

<template>
 <div class="w-full min-h-screen px-6 py-4 overflow-hidden" >
  <div class="flex space-x-3">
    <div v-for="(question,index) in questions" 
    class="w-full h-2 bg-gray-200 rounded-full"
    :class="`${question.selected ? 'bg-gray-400' :''}`"
    ></div>
  </div>
  <button v-if="!isCompleted" 
    @click="back"
  class="px-6 py-3 mt-6 text-white bg-red-500 rounded-md ">back</button>


  <div  v-if="!isCompleted" class="flex items-center justify-center mt-24">
    <div class="space-y-6" >
      <h1 class="mb-6 text-2xl font-bold text-center">{{GetCurrentQuestion.title}}</h1>
      <div class="space-y-6 w-96" >
        <div class="px-6 py-3 space-x-4 bg-gray-300 rounded-full " v-for="(option,index) in GetCurrentQuestion.options" 
        :key="index">
          <input type="radio" :value="index"
          v-model="GetCurrentQuestion.selected"
          @change="setAnswer"
           :name="GetCurrentQuestion.id"/>
          <span>{{option}}</span>
        </div>
       
      </div>
      <button @click="nextQuestion" class="block py-3 mx-auto mt-6 text-white bg-blue-500 rounded-full px-14 ">
      {{GetCurrentQuestion.index==questions.length-1?
      'Finish': 'Next'}}</button>
    </div>
  </div>
  <div v-else class="mt-12 text-xl text-center">
    <h3 class="font-bold ">Quiz completed</h3>
    <p>Score : {{score}}/{{questions.length}}</p>
  </div>
 </div>
</template>

