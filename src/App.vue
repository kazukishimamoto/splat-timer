<script setup lang="ts">
import Timer from './components/Timer.vue'
</script>

<template>
  <header>
    <h1>Splat Timer</h1>
    <p class="description">
      Splat Timer
      は、スプラトゥーンのナワバリバトルで残り時間を把握するためのタイマーです。
      ナワバリバトルでは、塗り状況と残り時間を適宜把握することが重要なため、残り時間と試合運びのポイントをアナウンスして勝てるようにサポートします。
    </p>
  </header>

  <main>
    <Timer />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
  text-align: center;
  margin-top: 1rem;
  color: #ffffff; /* 白色 */
  background-color: #333333; /* ダークグレー */
  width: 100%;
  padding: 1rem 0;
  box-sizing: border-box;
  max-width: 600px;
  margin: 0 auto;
}

.description {
  margin: 1rem 0;
  padding: 0 1rem;
  color: #cccccc; /* ライトグレー */
}

main {
  display: flex;
  align-items: center;
  flex-direction: column;
  height: calc(100vh - 4rem); /* ヘッダーの高さを引いた高さ */
  background-color: #333333; /* ダークグレー */
  width: 100%;
  padding: 0 1rem;
  box-sizing: border-box;
  max-width: 600px;
  margin: 0 auto;
}

@media (min-width: 768px) {
  main {
    max-width: 600px;
    margin: 0 auto;
  }
}
</style>
