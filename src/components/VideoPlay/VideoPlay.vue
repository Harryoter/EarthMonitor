<template>
  <div
    @mouseenter="videoMouseenter"
    @mouseleave="videoMouseleave"
    class="video-container"
    :style="[{ width: videoWidth }, { height: videoHeight }]"
    ref="video-container"
  >
    <video :src="videoSrc" ref="video" :poster="poster"></video>
    <div
      class="iconfont icon-play play"
      @click="playVideo"
      v-if="this.canPlay"
      :style="[{ color: themeColor }, { border: '6px solid ' + themeColor }]"
    ></div>
    <div
      :class="['controls', { 'controls-active': !this.canPlay && inVideo }]"
      :style="[{ color: themeColor }]"
    >
      <div
        :class="[
          'iconfont',
          { 'icon-pause': !pause && !end },
          { 'icon-play': pause && !end },
          { 'icon-shuaxin': end },
        ]"
        @click="playPauseVideo"
      ></div>
      <div
        class="progress"
        ref="progress"
        @click="handleProgress($event)"
        :style="[{ 'background-color': themeColor }]"
      >
        <span
          :style="[
            { width: progressValue + 'px' },
            { 'background-color': progressColor },
          ]"
        ></span>
      </div>
      <div class="iconfont icon-volume" @click="handleVolume">
        <div
          :class="[
            'volumeProgress',
            { 'volumeProgress-active': volumeProgress },
          ]"
          :style="[{ 'background-color': themeColor }]"
        >
          <span
            v-for="(item, index) in volumeProgressList"
            :key="item"
            @click.stop="handleVolumeProgress(index)"
            :style="[
              {
                'background-color':
                  (volumeProgressList.length - index) /
                    volumeProgressList.length <=
                  volume
                    ? progressColor
                    : themeColor,
              },
              {
                'border-top':
                  index === 0
                    ? 'none'
                    : (volumeProgressList.length - index) /
                        volumeProgressList.length <=
                      volume
                    ? '1px solid ' + themeColor
                    : '1px solid ' + progressColor,
              },
            ]"
          ></span>
        </div>
      </div>
      <div class="iconfont icon-screen" @click="handleScreen"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VideoPlay',
  props: {
    option: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  computed: {
    poster() {
      return this.option.poster || ''
    },
    loopPlay() {
      return this.option.loopPlay || false
    },
    dragProgress() {
      return this.option.dragProgress || true
    },
    videoWidth() {
      return this.option.width || '100%'
    },
    videoHeight() {
      return this.option.height || '100%'
    },
    videoSrc() {
      return this.option.src || '../'
    },
    videoHover() {
      return this.option.hover || true
    },
    themeColor() {
      return this.option.themeColor || '#fff'
    },
    progressColor() {
      return this.option.progressColor || '#ccc'
    }
  },
  data() {
    return {
      video: '',
      videoContainer: '',
      progress: '',
      canPlay: true,
      pause: false,
      progressValue: 0,
      progressTimer: 100,
      volume: 0.4,
      volumeProgress: false,
      volumeProgressList: [1, 2, 3, 4, 5],
      handleVolumeTimer: 3000,
      timer: null,
      volumeTimer: null,
      screen: false,
      end: false,
      inVideo: false
    }
  },
  mounted() {
    this.videoContainer = this.$refs['video-container']
    this.video = this.$refs.video
    this.video.volume = this.volume
    this.progress = this.$refs.progress
    this.video.addEventListener('ended', () => {
      this.pause = true
      this.end = true
      this.inVideo = true
      this.setProgress()
      clearInterval(this.timer)
      setTimeout(() => {
        this.progressValue = 0
        if (this.loopPlay) {
          this.end = false
          this.pause = false
          this.video.play()
          this.timer = setInterval(this.setProgress, this.progressTimer)
        }
      }, this.progressTimer)
    })
  },
  methods: {
    videoMouseenter() {
      this.inVideo = true
    },
    videoMouseleave() {
      this.inVideo = !!this.pause
      if (!this.videoHover) {
        clearTimeout(this.volumeTimer)
        this.volumeProgress = false
      }
      if (this.videoHover) this.inVideo = true
    },
    handleScreen() {
      if (!this.screen) {
        this.screen = true
        this.videoContainer.style.width = '100vw'
        this.videoContainer.style.height = '100vh'
        if (this.videoContainer.requestFullscreen) {
          this.videoContainer.requestFullscreen()
        } else if (this.videoContainer.webkitRequestFullscreen) {
          this.videoContainer.webkitRequestFullscreen()
        } else if (this.videoContainer.mozRequestFullScreen) {
          this.videoContainer.mozRequestFullScreen()
        } else if (this.videoContainer.msRequestFullscreen) {
          this.videoContainer.msRequestFullscreen()
        }
      } else {
        this.screen = false
        this.videoContainer.style.width = this.videoWidth
        this.videoContainer.style.height = this.videoHeight
        if (document.exitFullscreen) {
          document.exitFullscreen()
        } else if (document.webkitExitFullscreen) {
          document.webkitExitFullscreen()
        } else if (document.msExitFullscreen) {
          document.msExitFullscreen()
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen()
        }
      }
    },
    handleVolumeProgress(index) {
      clearTimeout(this.volumeTimer)
      this.video.volume = this.volume = (this.volumeProgressList.length - index) / this.volumeProgressList.length
      this.volumeTimer = setTimeout(() => {
        this.volumeProgress = false
      }, this.handleVolumeTimer)
    },
    handleVolume(event) {
      if (this.canPlay) return false
      if (this.volumeProgress) {
        clearTimeout(this.volumeTimer)
        this.volumeProgress = false
        return false
      }
      this.volumeProgress = true
      this.volumeTimer = setTimeout(() => {
        this.volumeProgress = false
      }, this.handleVolumeTimer)
    },
    setProgress() {
      this.progressValue = (this.video.currentTime / this.video.duration) * this.progress.offsetWidth
    },
    playVideo() {
      this.inVideo = true
      if (this.video) {
        this.video.play()
        this.canPlay = false
        this.timer = setInterval(this.setProgress, this.progressTimer)
      }
    },
    playPauseVideo() {
      this.end = false
      if (!this.pause) {
        this.pause = true
        this.video.pause()
        clearInterval(this.timer)
      } else {
        this.pause = false
        this.video.play()
        this.timer = setInterval(this.setProgress, this.progressTimer)
      }
    },
    handleProgress(event) {
      if (!this.dragProgress) return false
      this.video.currentTime = (event.offsetX / this.progress.offsetWidth) * this.video.duration
      this.setProgress()
    }
  }
}
</script>
<style scoped>
@import url("./iconfont/iconfont.css");
video {
  width: 100%;
  height: 100%;
  object-fit: fill;
  display: block;
}
.video-container {
  position: relative;
  user-select: none;
  -webkit-user-select: none;
  overflow: hidden;
}
.video-container .play {
  width: 60px;
  height: 60px;
  position: absolute;
  top: 0px;
  left: 0px;
  bottom: 0px;
  right: 0px;
  margin: auto;
  line-height: 60px;
  text-indent: 5px;
  text-align: center;
  font-size: 30px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.5s ease;
}
.video-container .controls {
  width: 100%;
  position: absolute;
  padding: 5px 0;
  bottom: 0px;
  transform: translateY(50px);
  transition: all 1s ease;
  box-sizing: border-box;
}
.video-container .controls-active {
  transform: translateY(0px);
}
.video-container .controls .iconfont {
  width: 40px;
  line-height: 30px;
  height: 30px;
  cursor: pointer;
  float: left;
  text-align: center;
  font-size: 25px;
  position: relative;
  z-index: 3;
}
.video-container .controls .progress {
  width: calc(100% - 120px);
  height: 12px;
  margin-top: 9px;
  float: left;
  overflow: hidden;
  cursor: pointer;
}
.video-container .controls .progress span {
  height: 100%;
  display: block;
  transition: all 0.1s ease;
}
.video-container .controls .volumeProgress {
  width: 15px;
  height: 100px;
  overflow: hidden;
  margin: 0 auto;
  position: relative;
  top: -130px;
  left: 0px;
  transition: all 1s ease;
  z-index: 2;
  opacity: 0;
  cursor: auto;
}
.video-container .controls .volumeProgress-active {
  opacity: 1;
  cursor: pointer;
}
.video-container .controls .volumeProgress span {
  width: 100%;
  height: 20%;
  box-sizing: border-box;
  display: block;
}
</style>
