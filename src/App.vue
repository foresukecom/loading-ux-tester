<template>
  <div class="min-h-screen bg-gray-50">
    <!-- メインエリア -->
    <div class="flex flex-col items-center justify-center min-h-screen px-4">
      <!-- タイトル -->
      <h1 class="text-4xl font-bold text-gray-800 mb-4">Loading UX Tester</h1>
      <p class="text-lg text-gray-600 mb-2 text-center max-w-2xl">
        ローディング時間とエフェクトがUXに与える影響を体感してみましょう
      </p>
      <p class="text-sm text-gray-500 mb-12 text-center">
        「同じ待ち時間でも、エフェクトがあると体感時間が短くなる」という仮説を検証
      </p>
      
      <!-- ローディング開始ボタン -->
      <button 
        @click="startLoading"
        :class="buttonClasses"
        class="text-xl font-bold py-6 px-12 rounded-lg transition-all duration-300 transform hover:scale-105 shadow-lg mb-16"
        :disabled="isLoading"
      >
        {{ buttonText }}
      </button>

      <!-- オプションエリア -->
      <div class="w-full max-w-md space-y-8">
        <!-- 時間設定スライダー -->
        <div class="bg-white p-6 rounded-lg shadow-md">
          <label class="block text-sm font-medium text-gray-700 mb-4">
            ローディング時間: {{ loadingTime }}秒
          </label>
          <input
            type="range"
            min="0"
            max="10"
            step="0.1"
            v-model="loadingTime"
            class="w-full h-3 bg-gray-200 rounded-lg appearance-none cursor-pointer slider"
          >
          <div class="flex justify-between text-xs text-gray-500 mt-2">
            <span>0秒</span>
            <span>5秒</span>
            <span>10秒</span>
          </div>
        </div>

        <!-- エフェクト選択 -->
        <div class="bg-white p-6 rounded-lg shadow-md">
          <label class="block text-sm font-medium text-gray-700 mb-4">
            ローディングエフェクト
          </label>
          <div class="grid grid-cols-2 gap-3">
            <button
              v-for="effect in effects"
              :key="effect.id"
              @click="selectedEffect = effect.id"
              :class="[
                'py-3 px-4 rounded-lg border-2 transition-all text-sm font-medium',
                selectedEffect === effect.id
                  ? 'border-blue-500 bg-blue-50 text-blue-700'
                  : 'border-gray-200 bg-white text-gray-700 hover:border-gray-300'
              ]"
            >
              {{ effect.name }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- ローディングオーバーレイ -->
    <div 
      v-if="isLoading"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    >
      <div class="bg-white rounded-lg p-8 max-w-sm w-full mx-4">
        <component :is="currentEffectComponent" :duration="loadingTime * 1000" />
      </div>
    </div>

    <!-- 結果サマリーカード -->
    <div 
      v-if="showResult"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
      @click="hideResult"
    >
      <div class="bg-white rounded-lg p-8 max-w-sm w-full mx-4 transform transition-all">
        <h3 class="text-xl font-bold text-gray-800 mb-4">体験結果</h3>
        <div class="space-y-2 text-gray-600">
          <p><span class="font-medium">待ち時間:</span> {{ lastLoadingTime }}秒</p>
          <p><span class="font-medium">エフェクト:</span> {{ lastEffectName }}</p>
        </div>
        <p class="text-sm text-gray-500 mt-4">クリックして閉じる</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import LoadingNone from './components/LoadingNone.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'
import LoadingProgress from './components/LoadingProgress.vue'
import LoadingQuotes from './components/LoadingQuotes.vue'
import LoadingProcess from './components/LoadingProcess.vue'
import LoadingHumor from './components/LoadingHumor.vue'

export default {
  name: 'App',
  components: {
    LoadingNone,
    LoadingSpinner,
    LoadingProgress,
    LoadingQuotes,
    LoadingProcess,
    LoadingHumor
  },
  setup() {
    const loadingTime = ref(2.0)
    const selectedEffect = ref('none')
    const isLoading = ref(false)
    const showResult = ref(false)
    const lastLoadingTime = ref(0)
    const lastEffectName = ref('')

    const effects = [
      { id: 'none', name: '何も表示しない' },
      { id: 'spinner', name: 'スピナー' },
      { id: 'progress', name: 'プログレスバー' },
      { id: 'quotes', name: '偉人の名言' },
      { id: 'process', name: '内部処理表示' },
      { id: 'humor', name: 'ユーモア・ジョーク' }
    ]

    const buttonClasses = computed(() => {
      if (loadingTime.value <= 2.5) {
        return 'bg-green-500 hover:bg-green-600 text-white'
      } else if (loadingTime.value <= 4.0) {
        return 'bg-yellow-500 hover:bg-yellow-600 text-white'
      } else {
        return 'bg-red-500 hover:bg-red-600 text-white'
      }
    })

    const buttonText = computed(() => {
      if (isLoading.value) {
        return 'ローディング中...'
      }
      
      if (loadingTime.value <= 2.5) {
        return 'ローディング開始 (快適)'
      } else if (loadingTime.value <= 4.0) {
        return 'ローディング開始 (許容範囲)'
      } else {
        return 'ローディング開始 (離脱リスク高)'
      }
    })

    const currentEffectComponent = computed(() => {
      const componentMap = {
        'none': 'LoadingNone',
        'spinner': 'LoadingSpinner',
        'progress': 'LoadingProgress',
        'quotes': 'LoadingQuotes',
        'process': 'LoadingProcess',
        'humor': 'LoadingHumor'
      }
      return componentMap[selectedEffect.value] || 'LoadingNone'
    })

    const startLoading = () => {
      isLoading.value = true
      lastLoadingTime.value = parseFloat(loadingTime.value)
      lastEffectName.value = effects.find(e => e.id === selectedEffect.value)?.name || ''

      setTimeout(() => {
        isLoading.value = false
        showResult.value = true
        
        // 5秒後に自動で結果を閉じる
        setTimeout(() => {
          showResult.value = false
        }, 5000)
      }, loadingTime.value * 1000)
    }

    const hideResult = () => {
      showResult.value = false
    }

    return {
      loadingTime,
      selectedEffect,
      isLoading,
      showResult,
      lastLoadingTime,
      lastEffectName,
      effects,
      buttonClasses,
      buttonText,
      currentEffectComponent,
      startLoading,
      hideResult
    }
  }
}
</script>

<style scoped>
.slider::-webkit-slider-thumb {
  appearance: none;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  background: #3b82f6;
  cursor: pointer;
}

.slider::-moz-range-thumb {
  height: 20px;
  width: 20px;
  border-radius: 50%;
  background: #3b82f6;
  cursor: pointer;
  border: none;
}
</style>