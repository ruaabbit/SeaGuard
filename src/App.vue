<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="bg-gray-50 min-h-screen">
    <header>
      <nav class="fixed top-0 w-full bg-white shadow-md z-50">
        <div class="max-w-[1440px] mx-auto px-8 h-16 flex items-center justify-between">
          <div class="flex items-center gap-2">
            <span class="text-2xl font-['Pacifico'] text-primary">OUC AI GROUP</span>
            <span class="text-lg font-medium">海洋卫士</span>
          </div>
          <div class="flex items-center space-x-8">
            <RouterLink to="/" class="text-primary hover:text-secondary whitespace-nowrap"
              >实时监测</RouterLink
            >
            <RouterLink to="/predict" class="text-gray-600 hover:text-secondary whitespace-nowrap"
              >预测分析</RouterLink
            >
            <RouterLink to="/history" class="text-gray-600 hover:text-secondary whitespace-nowrap"
              >历史数据</RouterLink
            >
            <RouterLink to="/report" class="text-gray-600 hover:text-secondary whitespace-nowrap"
              >数据报告</RouterLink
            >
          </div>
          <div class="flex items-center gap-4">
            <button
              class="bg-primary text-white px-4 py-2 !rounded-button hover:bg-opacity-90 whitespace-nowrap"
            >
              <i class="fas fa-user-plus mr-2"></i>加入行动
            </button>
            <div class="w-10 h-10 rounded-full bg-gray-200 flex items-center justify-center">
              <i class="fas fa-user text-gray-600"></i>
            </div>
          </div>
        </div>
      </nav>
    </header>

    <RouterView />
  </div>
</template>

<style>
/* Apply Tailwind's theme colors and border-radius */
.text-primary {
  color: theme('colors.primary');
}
.text-secondary {
  color: theme('colors.secondary');
}
.bg-primary {
  background-color: theme('colors.primary');
}
.bg-secondary {
  background-color: theme('colors.secondary');
}
.border-secondary {
  border-color: theme('colors.secondary');
}
.\!rounded-button {
  border-radius: 0.5rem !important;
}
</style>
