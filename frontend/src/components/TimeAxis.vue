<script setup>
import { ref } from 'vue'

const items = ref([
  '',
  '01:00',
  '02:00',
  '03:00',
  '04:00',
  '05:00',
  '06:00',
  '07:00',
  '08:00',
  '09:00',
  '10:00',
  '11:00',
  '12:00',
  '13:00',
  '14:00',
  '15:00',
  '16:00',
  '17:00',
  '18:00',
  '19:00',
  '20:00',
  '21:00',
  '22:00',
  '23:00',
])
</script>

<template>
  <div class="h-full">
    <!-- ヘッダー部分 -->
    <div class="h-16 border-b border-google-gray-200 bg-google-gray-50 flex items-center justify-center">
      <span class="text-xs text-google-gray-500 font-medium">時間</span>
    </div>
    
    <!-- 時間軸 -->
    <div v-for="(text, index) in items" :key="index" class="relative h-12 pr-3 text-right border-b border-google-gray-100">
      <span class="relative text-sm text-google-gray-500 -top-[13px]">
        {{ text }}
      </span>    
    </div>
  </div>    
</template>

<style scoped></style>
