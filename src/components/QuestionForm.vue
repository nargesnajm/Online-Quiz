<template>
  <div class="mt-6 ">
    <div class="question-box p-6 rounded-xl">
      <div class="mb-4">
        <div
  :class="[
    'text-lg font-semibold text-white mb-10',
    question.type === 'short' ? 'mt-25' : ''
  ]"
>
  {{ question.text }}
</div>
      </div>

      <div v-if="question.type === 'mcq'" class="grid gap-3">
        <label
          v-for="(opt, idx) in question.options"
          :key="idx"
          class="answer-option mb-1 flex items-center gap-3 p-3 rounded-lg cursor-pointer"
          :class="[labelClass(opt)]"
        >
          <input type="radio" :value="opt" v-model="localValue" class="form-radio" />
          <span class="text-white">{{ opt }}</span>
        </label>
      </div>

      <div v-else>
        <input
          v-model="localValue"
          type="text"
          placeholder="پاسخ خود را اینجا بنویسید"
          class="neon-input w-full p-3 mb-30 rounded-md"
          :class="{'border-red-400': showValidation && !modelValue}"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { watch, ref } from 'vue'

const props = defineProps({
  question: { type: Object, required: true },
  modelValue: { type: [String, null], default: null },
  showValidation: { type: Boolean, default: false }
})

const emits = defineEmits(['update:modelValue'])
const localValue = ref(props.modelValue)

watch(() => props.modelValue, (v) => localValue.value = v)
watch(localValue, (v) => emits('update:modelValue', v))

function labelClass(option) {
  if (!localValue.value) return props.showValidation && !props.modelValue ? 'border-red-400' : ''
  return props.showValidation && !props.modelValue ? 'border-red-400' : ''
}
</script>

<style scoped>
.question-box {
  border: 2px solid #ff00ff;
  box-shadow: 0 0 5px #ff00ff, 0 0 7px #ff00ff, 0 0 10px #ff00ff;
  background: transparent;

  /* 🔹 Added to make all boxes the same height */
  height: 396px; /* Adjust until all questions fit nicely */
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

/* Question text neon style */
.question-text {
  color: #ff00ff;
  text-shadow: 0 0 5px #ff00ff, 0 0 10px #ff00ff, 0 0 20px #ff00ff;
}

.answer-option {
  border: 2px solid #00ffff;
  box-shadow: 0 0 3px #00ffff, 0 0 5px #00ffff;
  transition: 0.3s;
}

.answer-option:hover {
  border: 2px solid #198383;
  box-shadow: 0 0 3px #198383, 0 0 5px #1c8a8a;
  transition: 0.3s;
}

.neon-input {
  border: 2px solid #00ffff;
  box-shadow: 0 0 3px #00ffff, 0 0 5px #00ffff;
  background: transparent;
  color: white;
}

.neon-input:active, .neon-input:focus {
  border-color: white;
}

.correct-option {
  border: 2px solid #39ff14;
  background-color: rgba(57, 255, 20, 0.2);
  color: white;
  box-shadow: 0 0 5px #39ff14, 0 0 10px #39ff14, 0 0 20px #39ff14;
}

.correct-option-outline {
  border: 2px solid #39ff14;
  box-shadow: 0 0 3px #39ff14, 0 0 5px #39ff14;
}

.wrong-option {
  border: 2px solid #ff3131;
  background-color: rgba(255, 49, 49, 0.2);
  color: white;
  box-shadow: 0 0 5px #ff3131, 0 0 10px #ff3131, 0 0 20px #ff3131;
}


</style>
