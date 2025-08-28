<script setup>
defineProps({
  categoryName: {
    type: String,
    default: '未分類',
  },
  time: {
    type: String,
    default: '時間未定',
  },
})
</script>

<template>
  <div class="absolute inset-x-0 bg-google-blue-100 hover:bg-google-blue-200 border-l-4 border-google-blue-500 z-10 p-3 rounded-google shadow-calendar-event hover:shadow-google-hover transition-all duration-200 cursor-pointer min-h-6 mx-1">
    <div class="flex flex-col h-full justify-center">
      <p class="font-medium text-sm text-google-gray-800 truncate leading-tight">{{ categoryName }}</p>
      <p class="text-xs text-google-gray-600 mt-1 truncate">{{ time }}</p>
    </div>
  </div>
</template>
