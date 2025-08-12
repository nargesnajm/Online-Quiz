<template>
    <div class="mt-4">
      <div class="flex items-center justify-between mb-2">
        <div class="text-sm font-medium text-slate-500"
        style="text-shadow:  0 0 8px #0fc8f7;">پیشرفت</div>
        <div class="text-sm font-medium text-slate-500"
        style="text-shadow:  0 0 10px #0fc8f7;">{{ percent }}%</div>
      </div>
  
      <div class="progress-wrapper">
        <div
          class="progress-bar"
          :class="colorClass"
          :style="{ width: percent + '%' }"
        ></div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  const props = defineProps({ answeredCount: { type: Number, required: true }, total: { type: Number, required: true } })
  
  const percent = computed(() => {
    if (props.total === 0) return 0
    
    const p = Math.round((props.answeredCount / props.total) * 100)
    return p
  })
  
  const colorClass = computed(() => {
    if (percent.value <= 50) return 'bg-[#ff073A]'
    if (percent.value <= 80) return 'bg-[#dcff1e]'
    return 'bg-[#66FF00]'
  })
  </script>