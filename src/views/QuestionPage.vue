<script setup>
import { onMounted, ref } from 'vue';
import useAPI from '@/composables/useAPI';
import { useRoute } from 'vue-router';
import BaseTitle from '@/components/BaseTitle.vue';

const api = useAPI()
const question = ref(null)
const route = useRoute()
const answer= ref([])
onMounted(async () => {
  question.value = await api.getQuestion(route.params.id)

  answers.value.push({
    id: answers.value.length,
    correct: true,
    answer: question.value.correct_answer
  })

  question.value.incorrect_answers.map((wrong_answer) => {
    answers.value.push({
      id: answers.value.length,
      correct: false,
      answer: wrong_answer
    })

  })

  answers.value = shuffle(answers.value)
  //console.log(question.value)

})

const shuffle = (array) => {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    //swap
    [array[i], array[j]] = [array[j], array[i]];
  }

  return array
}

</script>

<template>

<div v-if="question" class="flex h-full w-full flex-col items-center gap-8 p-10">
<BaseTitle>{{  question.category }}</BaseTitle>
<!-- {{  question.question  }} -->

<div v-html="question.question" class="text-center text-2x1 font-bold"></div>
<div class="grid w-full flex-grow grid-cols-2 gap-8">
  <div v-for="answer in answers" 
  v-html="answer.answer" 
  :key="answer.id" 
  class="bg-green-500 flex items-center jsutify-center text-4x1 rounded-leg text-white py-10 px-2">
  
  </div>

    </div>
  </div>
<div v-else class="">
  Loading...
</div>

</template>
