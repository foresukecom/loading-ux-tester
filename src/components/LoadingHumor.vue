<template>
  <div class="text-center py-8">
    <div class="text-gray-600 text-lg mb-6">
      ローディング中...
    </div>
    
    <!-- ユーモア・ジョーク表示 -->
    <div class="max-w-sm mx-auto bg-yellow-50 rounded-lg p-4 mb-4 border-l-4 border-yellow-400">
      <div class="flex items-start space-x-3">
        <div class="flex-shrink-0 text-yellow-500 text-lg">
          {{ currentJoke.emoji }}
        </div>
        <div class="flex-1 text-left">
          <p class="text-yellow-800 text-sm font-medium leading-relaxed">
            {{ currentJoke.text }}
          </p>
          <div class="text-xs text-yellow-600 mt-2 italic" v-if="currentJoke.subtitle">
            {{ currentJoke.subtitle }}
          </div>
        </div>
      </div>
    </div>
    
    <!-- スピナーアニメーション -->
    <div class="inline-block mb-4">
      <div class="w-6 h-6 border-2 border-yellow-500 border-t-transparent rounded-full animate-spin"></div>
    </div>
    
    <div class="text-gray-400 text-xs">
      {{ jokeIndex + 1 }} / {{ jokes.length }} - ちょっと一息
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'

export default {
  name: 'LoadingHumor',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  },
  setup(props) {
    const jokeIndex = ref(0)
    let interval = null

    const jokes = [
      {
        emoji: '🔥',
        text: 'サーバーを温めています...',
        subtitle: '電気代、心配です。'
      },
      {
        emoji: '🐌',
        text: 'カタツムリより速く処理中...',
        subtitle: 'ゆっくりですが確実に！'
      },
      {
        emoji: '☕',
        text: 'コーヒーを飲む時間ができました！',
        subtitle: 'せっかくなのでリラックスしましょう'
      },
      {
        emoji: '🤖',
        text: 'ロボットが一生懸命計算中...',
        subtitle: '0と1の世界で格闘中'
      },
      {
        emoji: '🌍',
        text: '豆知識: 世界初のWebサイトは1991年に公開されました',
        subtitle: 'まだ30年ちょっとの歴史なんです'
      },
      {
        emoji: '🧠',
        text: 'AIが考えごとをしています...',
        subtitle: '人工知能も時には悩むのです'
      },
      {
        emoji: '🎯',
        text: 'ベストな結果を探しています...',
        subtitle: '妥協はしません！'
      },
      {
        emoji: '🎪',
        text: 'データのジャグリング中...',
        subtitle: '落とさないよう気をつけて'
      }
    ]

    const currentJoke = computed(() => jokes[jokeIndex.value])

    onMounted(() => {
      // ランダムな開始位置
      jokeIndex.value = Math.floor(Math.random() * jokes.length)
      
      // 2.5秒ごとにジョークを切り替え
      const switchInterval = Math.max(2500, props.duration / 3)
      
      interval = setInterval(() => {
        jokeIndex.value = (jokeIndex.value + 1) % jokes.length
      }, switchInterval)
    })

    onUnmounted(() => {
      if (interval) {
        clearInterval(interval)
      }
    })

    return {
      jokeIndex,
      jokes,
      currentJoke
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