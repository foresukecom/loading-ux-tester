<template>
  <div class="text-center py-8">
    <div class="inline-block animate-spin rounded-full h-16 w-16 border-4 border-blue-500 border-t-transparent"></div>
    <div class="text-gray-600 text-lg mt-4">
      ローディング中...
    </div>
    <div class="text-gray-400 text-sm mt-2">
      しばらくお待ちください
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoadingSpinner',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  }
}
</script>

<style scoped>
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.animate-spin {
  animation: spin 1s linear infinite;
}
</style>