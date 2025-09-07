<template>
  <div class="text-center py-8">
    <div class="text-gray-600 text-lg mb-6">
      ローディング中...
    </div>
    
    <!-- プログレスバー -->
    <div class="w-full bg-gray-200 rounded-full h-3 mb-4">
      <div 
        class="bg-blue-500 h-3 rounded-full transition-all duration-100 ease-out"
        :style="{ width: progress + '%' }"
      ></div>
    </div>
    
    <div class="text-gray-500 text-sm">
      {{ Math.round(progress) }}%
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'

export default {
  name: 'LoadingProgress',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  },
  setup(props) {
    const progress = ref(0)
    let interval = null

    onMounted(() => {
      const incrementTime = 50 // 50msごとに更新
      const incrementValue = 100 / (props.duration / incrementTime)

      interval = setInterval(() => {
        if (progress.value < 100) {
          progress.value = Math.min(progress.value + incrementValue, 100)
        }
      }, incrementTime)
    })

    onUnmounted(() => {
      if (interval) {
        clearInterval(interval)
      }
    })

    return {
      progress
    }
  }
}
</script>