<template>
  <div class="timer-root">
    <div class="timer-title">番茄钟</div>
    <div class="timer-countdown">{{ countdown }}</div>
    <div class="timer-btns">
      <button class="timer-btn" @click="startTimer">开始</button>
      <button class="timer-btn" @click="resetTimer">重置</button>
    </div>
    <audio id="alarm-sound" src="/ding.mp3"></audio>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 2700, // 25分钟（25*60秒）
      interval: null // 定时器
    }
  },
  computed: {
    minutes() {
      return Math.floor(this.time / 60).toString().padStart(2, '0');
    },
    seconds() {
      return (this.time % 60).toString().padStart(2, '0');
    },
    countdown() {
      return `${this.minutes}:${this.seconds}`;
    }
  },
  methods: {
    startTimer() {
      const audio = document.getElementById('bg-music');
      if (audio) {
        audio.currentTime = 0;
        audio.play();
      }
      this.interval = setInterval(() => {
        this.time--;
        if (this.time === 0) {
          alert('番茄钟完成！休息一下吧~');
          const audio = document.getElementById('bg-music');
          if (audio) {
            audio.pause();
          }
          const alarm = document.getElementById('alarm-sound');
          if (alarm) {
            alarm.currentTime = 0;
            alarm.play();
          }
          this.resetTimer();
        }
      }, 1000); // 每秒减1
    },
    resetTimer() {
      clearInterval(this.interval);
      const audio = document.getElementById('bg-music');
      if (audio) {
        audio.currentTime = 0;
        audio.play();
      }
      this.time = 2700;
    }
  }
}
</script>

<style scoped>
.timer-root {
  width: 100%;
  height: 100%;
  min-width: 0;
  min-height: 0;
  background: rgba(255,255,255,0.32);
  border-radius: 0 0 24px 0; /* 只右下角圆角 */
  box-shadow: 0 8px 32px rgba(0,0,0,0.10);
  border: none; /* 去掉分割线 */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* 垂直居中 */
  padding: 56px 40px;
  box-sizing: border-box;
  color: #333;
  font-size: 32px;
  backdrop-filter: blur(6px);
}

.timer-title {
  font-size: 38px;
  font-weight: bold;
  margin-bottom: 38px;
  color: #333;
  letter-spacing: 2px;
}

.timer-countdown {
  font-size: 72px;
  font-weight: bold;
  letter-spacing: 6px;
  margin-bottom: 38px;
  color: #333;
}

.timer-btns {
  display: flex;
  gap: 24px;
}

.timer-btn {
  background: linear-gradient(90deg, #a1c4fd 0%, #c2e9fb 100%);
  border: none;
  border-radius: 14px;
  padding: 16px 38px;
  color: #333;
  font-size: 22px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.2s, transform 0.2s;
}
.timer-btn:hover {
  background: linear-gradient(90deg, #89f7fe 0%, #66a6ff 100%);
  transform: translateY(-2px) scale(1.04);
}
</style>