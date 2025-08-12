<template>
    <div class="text-center py-12">
      <p class="text-slate-500 mb-20 font-semibold text-lg ">آزمون شامل {{ total }} تا سواله. قبل از رفتن به سوال بعدی، جواب رو وارد و یا انتخاب کن.</p>
      <button @click="$emit('start')" class="px-8 py-3 bg-[#46CB18] text-white rounded-full font-semibold shadow hover:bg-[#66FF00] transition neon-button" >شروع آزمون</button>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue'
  const props = defineProps({ total: { type: Number, default: 10 } })
  </script>

  <style>
   
   .neon-button {
  box-shadow:  0 0 10px #46dd14, 0 0 10px #00ff99;
  
 
  
}

  </style>