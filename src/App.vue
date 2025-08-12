<template>
  <div class="min-h-screen flex items-center justify-center p-6 bg-slate-950"
  style="background-image: url('/src/assets/digital3.jpg'); background-size: cover; background-position: center;"
  >
    <div class="w-full max-w-3xl">
      <div class="start-page bg-slate-900 rounded-2xl shadow-2xl p-6 md:p-10 ">
        <!-- Header -->
        <div class="flex items-center justify-between mb-6" v-if="!showResult">
          <h1 class="text-[#ff9535] text-2xl md:text-3xl font-bold  " style="text-shadow:  0 0 8px #ff9837;">آزمون آنلاین — مهارت‌هاتو بسنج</h1>
          <div v-if="started" class="text-sm text-slate-300">
    سوال {{ currentIndex+1 }} از {{ totalQuestions }}
  </div>
        </div>

        <!-- Start page -->
        <StartPage v-if="!started && !showResult" @start="startQuiz" />

        <!-- Quiz section -->
        <div v-else-if="!showResult">
          <!-- Progress -->
          <ProgressBar :answeredCount="answeredCount" :total="totalQuestions" />

          <!-- Question form -->
          <QuestionForm
            :question="questions[currentIndex]"
            v-model="answers[currentIndex]"
            :showValidation="showValidation && !answers[currentIndex]"
          />

          <!-- Controls -->
          <div class="mt-6 flex items-center justify-between">
            <button
              class="px-5 py-2 rounded-lg bg-[#b10eeb] text-white font-semibold shadow hover:bg-[#be2bf1] transition"
              
              @click="prev"
              :disabled="currentIndex===0"
            >قبلی</button>

            <div class="flex items-center gap-3">
              <button
                v-if="currentIndex < totalQuestions-1"
                class="px-5 py-2 rounded-lg bg-[#b10eeb] text-white font-semibold shadow hover:bg-[#be2bf1] transition"
                @click="next"
              >بعدی</button>

              <button
                v-else
                class="px-5 py-2 rounded-lg bg-[#46CB18] text-white font-semibold shadow hover:bg-[#66FF00] transition"
                @click="submitQuiz"
              >ارسال آزمون</button>
            </div>
          </div>


          <p v-if="showValidation && !answers[currentIndex]" class="mt-7 text-base font-semibold text-[#e3165a] ">
              🔶  لطفاً قبل از رفتن به سوال بعد پاسختونو وارد و یا انتخاب کنید.
          </p>
        </div>

        <Result v-if="showResult" :questions="questions" :answers="answers" @restart="restart" />

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import StartPage from './components/StartPage.vue'
import QuestionForm from './components/QuestionForm.vue'
import ProgressBar from './components/ProgressBar.vue'
import Result from './components/Result.vue'
import questions from "./components/Data.js"

const totalQuestions = questions.length
const started = ref(false)
const currentIndex = ref(0)
const answers = ref(Array(totalQuestions).fill(null))
const showValidation = ref(false)
const showResult = ref(false)

const answeredCount = computed(() => answers.value.filter(a => a !== null && a !== '').length)

function startQuiz() {
  started.value = true
  currentIndex.value = 0
  showResult.value = false
}

function prev() {
  if (currentIndex.value > 0) {
    currentIndex.value--
    showValidation.value = false
  }
}

function next() {
  if (!answers.value[currentIndex.value] || answers.value[currentIndex.value] === '') {
    showValidation.value = true
    return
  }
  showValidation.value = false
  if (currentIndex.value < totalQuestions - 1) currentIndex.value++
}

function submitQuiz() {
  const firstEmpty = answers.value.findIndex(a => a === null || a === '')
  if (firstEmpty !== -1) {
    currentIndex.value = firstEmpty
    showValidation.value = true
    return
  }
  showResult.value = true
}

function restart() {
  started.value = false
  answers.value = Array(totalQuestions).fill(null)
  currentIndex.value = 0
  showResult.value = false
  showValidation.value = false
}
</script>
  <style>
.start-page {
  border: 2px solid #ff00ff;
  box-shadow: 0 0 10px #ff00ff, 0 0 15px #ff00ff, 0 0 30px #ff00ff;
  
}


</style>