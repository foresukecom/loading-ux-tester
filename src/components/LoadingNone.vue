<template>
  <div class="text-center py-8">
    <div class="text-gray-500 text-lg">
      ローディング中...
    </div>
    <div class="text-gray-400 text-sm mt-2">
      (何も表示しない)
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoadingNone',
  props: {
    duration: {
      type: Number,
      default: 3000
    }
  }
}
</script>