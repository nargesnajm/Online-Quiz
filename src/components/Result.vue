<template>
  <div class="mt-6 p-6 bg-slate-800 text-white rounded-xl shadow-lg result-wrapper">
    
  
    <div v-if="score === total" class="text-center">
      <h2 class="text-3xl font-bold mb-8 text-green-400" style="text-shadow: 0 0 12px #00ff99;">
        🎉 آفرین ! همه سوالاتو درست جواب دادی!
      </h2>
    </div>

 
    <div v-else>
      <h2 class="text-3xl font-bold mb-8 text-[#ff9535]"
        style="text-shadow:  0 0 8px #ff9837;">نتیجهٔ آزمون</h2>

      <div class="mb-8">
        <div class="text-lg" style="text-shadow:  0 0 5px #37e8ff;">
          نمره: <span class="font-semibold">{{ score }} / {{ total }}</span>
        </div>
        <div class="text-lg" style="text-shadow:  0 0 5px #37e8ff;">
          درصد: <span class="font-semibold">{{ percent }}%</span>
        </div>
      </div>

      <div class="grid gap-3 mt-4">
        <div 
          v-for="(q, idx) in questions" 
          :key="q.id" 
          class="p-4 rounded border"
          :class="{
            'border-green-400': isCorrect(idx),
            'border-[#ff4242]': !isCorrect(idx)
          }"
        >
          <div class="font-semibold">{{ idx+1 }}. {{ q.text }}</div>
          <div class="mt-5 mb-2 text-sm">
            پاسخ شما: <span class="font-semibold">{{ answers[idx] || '-' }}</span>
          </div>
          <div class="mt-1 text-sm text-green-400">
            پاسخ صحیح: <span class="font-semibold">{{ q.correct }}</span>
          </div>
        </div>
      </div>
    </div>


    <div class="mt-6 flex gap-3 justify-end">
      <button @click="$emit('restart')" 
        class="px-5 py-2 rounded-lg bg-[#b10eeb] text-white font-semibold shadow hover:bg-[#be2bf1] transition">
        تلاش مجدد
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
const props = defineProps({ questions: Array, answers: Array })

const total = computed(() => props.questions.length)
const score = computed(() => {
  let s = 0
  props.questions.forEach((q, idx) => {
    const a = (props.answers[idx] || '').toString().trim()
    if (!a) return
    if (q.type === 'mcq') {
      if (a === q.correct) s++
    } else {
      const correct = q.correct.toString().toLowerCase()
      if (a.toLowerCase().includes(correct) || correct.includes(a.toLowerCase())) s++
    }
  })
  return s
})

const percent = computed(() => Math.round((score.value / total.value) * 100))

function isCorrect(idx) {
  const a = (props.answers[idx] || '').toString().trim()
  const q = props.questions[idx]
  if (!a) return false
  if (q.type === 'mcq') {
    return a === q.correct
  } else {
    const correct = q.correct.toString().toLowerCase()
    return a.toLowerCase().includes(correct) || correct.includes(a.toLowerCase())
  }
}
</script>

<style>
.result-wrapper {
  border: 1px solid #6d6c6d;
  box-shadow: 0 0 2px #6d6c6d, 0 0 12px #6d6c6d, 0 0 20px #6d6c6d;
}
</style>
