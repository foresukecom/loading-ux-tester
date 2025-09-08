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
      // 嘘の進捗系（サーバー・システム関連）
      {
        emoji: '🔥',
        text: 'サーバーを温めています...',
        subtitle: '電気代、心配です。'
      },
      {
        emoji: '❄️',
        text: 'サーバールームのエアコンを調整中...',
        subtitle: '快適な温度でお待ちください'
      },
      {
        emoji: '🔧',
        text: 'ハムスターがホイールを回してます...',
        subtitle: '今日も元気に発電中！'
      },
      {
        emoji: '🍳',
        text: 'データを煮込んでいます...',
        subtitle: 'いい感じに仕上がってきました'
      },
      {
        emoji: '🎪',
        text: 'データのジャグリング中...',
        subtitle: '落とさないよう気をつけて'
      },
      {
        emoji: '🎭',
        text: 'ファイルが変装を解いています...',
        subtitle: '本当の姿が見えてきました'
      },
      {
        emoji: '🎨',
        text: '結果に色を塗っています...',
        subtitle: 'きれいに仕上げますね'
      },
      {
        emoji: '🔍',
        text: 'バグが隠れていないかチェック中...',
        subtitle: 'みつけたら教えてください'
      },
      {
        emoji: '🧹',
        text: 'キャッシュのお掃除をしています...',
        subtitle: 'スッキリ！'
      },
      {
        emoji: '🏗️',
        text: 'レスポンスを建設中...',
        subtitle: '安全第一で作業してます'
      },
      
      // 動物・生物系の嘘進捗
      {
        emoji: '🐌',
        text: 'カタツムリより速く処理中...',
        subtitle: 'ゆっくりですが確実に！'
      },
      {
        emoji: '🐢',
        text: 'ウサギとカメの競争中...',
        subtitle: 'カメが勝ちそうです'
      },
      {
        emoji: '🦥',
        text: 'ナマケモノのペースで進行中...',
        subtitle: '急がば回れ、ですね'
      },
      {
        emoji: '🐝',
        text: 'ミツバチがデータを集めています...',
        subtitle: 'ブンブン、お疲れ様！'
      },
      {
        emoji: '🦋',
        text: 'さなぎから美しい結果が...',
        subtitle: '変身まであと少し'
      },
      
      // 食べ物・料理系
      {
        emoji: '☕',
        text: 'コーヒーを淹れる時間ができました！',
        subtitle: 'せっかくなのでリラックスしましょう'
      },
      {
        emoji: '🍞',
        text: 'データがパンのように膨らんでます...',
        subtitle: 'いい香りがしてきました'
      },
      {
        emoji: '🍝',
        text: 'アルデンテまであと少し...',
        subtitle: 'パスタもプログラムも茹で加減が大事'
      },
      {
        emoji: '🧀',
        text: 'チーズのように熟成中...',
        subtitle: '時間をかけた分、美味しくなります'
      },
      {
        emoji: '🍯',
        text: 'はちみつのようにとろ〜り処理中...',
        subtitle: '甘くて濃厚な結果をお届け'
      },
      
      // テクノロジー・AI系
      {
        emoji: '🤖',
        text: 'ロボットが一生懸命計算中...',
        subtitle: '0と1の世界で格闘中'
      },
      {
        emoji: '🧠',
        text: 'AIが深く考えています...',
        subtitle: '人工知能も時には悩むのです'
      },
      {
        emoji: '💫',
        text: 'アルゴリズムが宇宙の真理を探索中...',
        subtitle: '42が答えかもしれません'
      },
      {
        emoji: '⚡',
        text: '電子が光の速度で移動中...',
        subtitle: 'アインシュタインもびっくり'
      },
      {
        emoji: '🛸',
        text: '宇宙人がサーバーを点検中...',
        subtitle: '高度な技術で対応してます'
      },
      
      // 豆知識・トリビア系
      {
        emoji: '🌍',
        text: '豆知識: 世界初のWebサイトは1991年に公開されました',
        subtitle: 'まだ30年ちょっとの歴史なんです'
      },
      {
        emoji: '📚',
        text: '豆知識: 最初のバグは本物の虫でした',
        subtitle: '1947年、コンピューターに蛾が挟まってたそうです'
      },
      {
        emoji: '💾',
        text: '昔のハードディスクは冷蔵庫サイズでした',
        subtitle: '今やスマホの方が高性能です'
      },
      {
        emoji: '🎮',
        text: 'テトリスは世界で最も移植されたゲームです',
        subtitle: '落ちてくるブロックのように処理中'
      },
      
      // 作業・職人系
      {
        emoji: '🎯',
        text: 'ベストな結果を狙い撃ち中...',
        subtitle: '妥協はしません！'
      },
      {
        emoji: '🎨',
        text: '芸術家がピクセルを一つずつ描画中...',
        subtitle: 'モナリザ級の美しさを目指してます'
      },
      {
        emoji: '🔨',
        text: 'データを叩いて形を整えています...',
        subtitle: '職人の技をご覧ください'
      },
      {
        emoji: '✂️',
        text: '不要な部分をチョキチョキ中...',
        subtitle: 'スッキリした結果をお届け'
      },
      
      // 自然・天気系
      {
        emoji: '🌱',
        text: '結果が芽を出し始めました...',
        subtitle: '大きく育ちますように'
      },
      {
        emoji: '🌙',
        text: '夜なべしてデータを処理中...',
        subtitle: '朝には素敵な結果が'
      },
      {
        emoji: '⭐',
        text: '流れ星にお願い中...',
        subtitle: 'きっと願いが叶います'
      },
      {
        emoji: '🌈',
        text: '虹の橋を架けています...',
        subtitle: '色とりどりの結果をお届け'
      },
      
      // ユニーク・面白系
      {
        emoji: '🎪',
        text: 'サーカス団がデータを曲芸中...',
        subtitle: '息をのむパフォーマンス！'
      },
      {
        emoji: '🎭',
        text: '演劇の稽古をしています...',
        subtitle: '本番では最高の演技を'
      },
      {
        emoji: '🎵',
        text: 'データがオーケストラを奏でています...',
        subtitle: '美しいハーモニーをお楽しみに'
      },
      {
        emoji: '🎲',
        text: '運命のサイコロを振っています...',
        subtitle: 'いい目が出ますように'
      },
      {
        emoji: '🔮',
        text: '水晶玉で未来を占っています...',
        subtitle: '素晴らしい結果が見えます'
      },
      {
        emoji: '🧙‍♂️',
        text: '魔法使いが呪文を唱えています...',
        subtitle: 'アブラカダブラ！'
      },
      {
        emoji: '🏃‍♂️',
        text: 'マラソンランナーのように根性で処理中...',
        subtitle: 'ゴールまでもう少し！'
      },
      {
        emoji: '🎪',
        text: 'ピエロが風船を膨らませています...',
        subtitle: '割らないよう慎重に...'
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