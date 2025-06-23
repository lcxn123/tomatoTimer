<template>
  <div>
    <video autoplay loop muted playsinline id="bg-video"
      style="position:fixed;top:0;left:0;width:100vw;height:100vh;object-fit:cover;z-index:-1;">
      <source src="/sea.MP4" type="video/mp4" />
    </video>
    <audio
      id="bg-music"
      :src="musicList[currentMusic]"
      @ended="playNext"
      autoplay
      ref="audio"
    ></audio>
    <div class="main-layout">
      <div class="left-panel">
        <TodoList />
      </div>
      <div class="right-panel">
        <Timer />
      </div>
    </div>
  </div>
</template>

<script>
import Timer from './components/Timer.vue'
import TodoList from './components/TodoList.vue'

export default {
  components: { Timer, TodoList },
  data() {
    return {
      musicList: [
        '/music1.mp3',
        '/music2.mp3',
        '/music3.mp3'
      ],
      currentMusic: 0
    }
  },
  methods: {
    playNext() {
      // 切换到下一首，循环播放
      this.currentMusic = (this.currentMusic + 1) % this.musicList.length;
      // 重新播放
      this.$refs.audio.load();
      this.$refs.audio.play();
    }
  }
}
</script>

<style>
body {
  font-family: 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB', 'Arial', sans-serif;
  background: transparent;
  color: #222;
  margin: 0;
  padding: 0;
}

.main-layout {
  display: flex;
  flex-direction: column; /* 移动端改为垂直布局 */
  width: 100vw;
  height: 100vh;
  min-height: 100vh;
  box-sizing: border-box;
  padding: 20px;
  gap: 20px;
}

/* 响应式布局 - 桌面端 */
@media (min-width: 768px) {
  .main-layout {
    flex-direction: row;
  }
  
  .left-panel {
    flex: 0 0 30%;
    min-width: 280px;
  }
  
  .right-panel {
    flex: 0 0 70%;
    min-width: 400px;
  }
}

/* 移动端布局 */
@media (max-width: 767px) {
  .left-panel, .right-panel {
    width: 100%;
  }
}
</style>