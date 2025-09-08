<template>
  <div class="text-center py-8">
    <div class="text-gray-600 text-lg mb-6">
      処理中...
    </div>
    
    <!-- 処理ステップ表示 -->
    <div class="max-w-sm mx-auto bg-blue-50 rounded-lg p-4 mb-4">
      <div class="flex items-center space-x-3">
        <div class="flex-shrink-0">
          <div class="w-3 h-3 bg-blue-500 rounded-full animate-pulse"></div>
        </div>
        <div class="flex-1 text-left">
          <p class="text-blue-800 text-sm font-medium">
            {{ currentStep.text }}
          </p>
          <div class="text-xs text-blue-600 mt-1">
            ステップ {{ currentStepIndex + 1 }} / {{ processSteps.length }}
          </div>
        </div>
      </div>
    </div>
    
    <!-- 進捗インジケーター -->
    <div class="flex justify-center space-x-1">
      <div
        v-for="(step, index) in processSteps"
        :key="index"
        :class="[
          'w-2 h-2 rounded-full transition-colors duration-300',
          index <= currentStepIndex ? 'bg-blue-500' : 'bg-gray-300'
        ]"
      ></div>
    </div>
    
    <div class="text-gray-400 text-xs mt-4">
      透明性のある処理フィードバック
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'

export default {
  name: 'LoadingProcess',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  },
  setup(props) {
    const currentStepIndex = ref(0)
    let interval = null

    const processSteps = [
      { text: 'ユーザー情報を読み込んでいます...' },
      { text: 'データベースに接続しています...' },
      { text: '最新のデータを取得中...' },
      { text: 'セキュリティチェックを実行中...' },
      { text: 'データを整理しています...' },
      { text: '最終処理をしています...' },
      { text: 'もう少しで完了します...' }
    ]

    const currentStep = computed(() => processSteps[currentStepIndex.value])

    onMounted(() => {
      // プロセスステップの進行間隔を計算
      const stepInterval = Math.max(800, props.duration / processSteps.length)
      
      interval = setInterval(() => {
        if (currentStepIndex.value < processSteps.length - 1) {
          currentStepIndex.value++
        }
      }, stepInterval)
    })

    onUnmounted(() => {
      if (interval) {
        clearInterval(interval)
      }
    })

    return {
      currentStepIndex,
      processSteps,
      currentStep
    }
  }
}
</script>

<style scoped>
@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>