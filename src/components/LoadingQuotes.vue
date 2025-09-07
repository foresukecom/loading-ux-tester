<template>
  <div class="text-center py-8">
    <div class="text-gray-600 text-lg mb-6">
      ローディング中...
    </div>
    
    <!-- 偉人の名言 -->
    <div class="max-w-sm mx-auto bg-blue-50 rounded-lg p-4 mb-4">
      <blockquote class="text-blue-800 italic text-sm leading-relaxed">
        "{{ currentQuote.text }}"
      </blockquote>
      <cite class="text-blue-600 text-xs block mt-2 not-italic">
        — {{ currentQuote.author }}
      </cite>
    </div>
    
    <div class="text-gray-400 text-xs">
      {{ quoteIndex + 1 }} / {{ quotes.length }}
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'

export default {
  name: 'LoadingQuotes',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  },
  setup(props) {
    const quoteIndex = ref(0)
    let interval = null

    const quotes = [
      {
        text: "成功とは、失敗から失敗へと、情熱を失わずに進むことである。",
        author: "ウィンストン・チャーチル"
      },
      {
        text: "天才とは1%のひらめきと99%の努力である。",
        author: "トーマス・エジソン"
      },
      {
        text: "未来を予測する最良の方法は、それを創造することだ。",
        author: "ピーター・ドラッカー"
      },
      {
        text: "人生において最も困難なことは、自分自身を知ることである。",
        author: "ソクラテス"
      },
      {
        text: "問題を生み出したのと同じマインドでは、その問題を解決することはできない。",
        author: "アルベルト・アインシュタイン"
      },
      {
        text: "学習とは、学校で学んだことをすべて忘れた後に残るものである。",
        author: "アルベルト・アインシュタイン"
      }
    ]

    const currentQuote = computed(() => quotes[quoteIndex.value])

    onMounted(() => {
      // 名言をランダムに開始
      quoteIndex.value = Math.floor(Math.random() * quotes.length)
      
      // 2秒ごとに名言を切り替える
      const switchInterval = Math.max(2000, props.duration / 3)
      
      interval = setInterval(() => {
        quoteIndex.value = (quoteIndex.value + 1) % quotes.length
      }, switchInterval)
    })

    onUnmounted(() => {
      if (interval) {
        clearInterval(interval)
      }
    })

    return {
      quoteIndex,
      quotes,
      currentQuote
    }
  }
}
</script>