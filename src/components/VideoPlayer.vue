<template>
  <video-player
    class="video-player vjs-custom-skin"
    :playsinline="true"
    :options="playerOptions"
    @play="onPlayerPlay($event)"
    @pause="onPlayerPause($event)"
  ></video-player>
</template>
<script>
export default {
  name: 'BusImg',
  props: {
    videoSrc: {
      type: String
    },
    status: {
      type: Boolean
    }
  },
  computed: {
    // 视频播放
    playerOptions() {
      return {
        playbackRates: [0.7, 1.0, 1.5, 2.0], // 播放速度
        autoplay: true, // 如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 导致视频一结束就重新开始。
        preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: 'zh-CN',
        aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        techOrder: ['flash', 'html5'], // 兼容顺序
        flash: {
          hls: { withCredentials: false },
          swf: 'https://cdn.bootcss.com/videojs-swf/5.4.1/video-js.swf' // 引入静态文件swf
        },
        html5: { hls: { withCredentials: false } },
        sources: [{ // 流配置，数组形式，会根据兼容顺序自动切换
          type: 'rtmp/hls',
          src: this.videoSrc
        }],
        poster: '', // 你的封面地址
        // width: document.documentElement.clientWidth,
        notSupportedMessage: '此视频暂无法播放，请稍后再试', // 允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true // 全屏按钮
        }
      }
    }
  },
  methods: {
    onPlayerPlay(player) {
      this.$emit('update:status', true)
    },
    // 暂停回调
    onPlayerPause(player) {
      this.$emit('update:status', false)
    }
  }
}
</script>
<style scoped lang="scss">
::v-deep button:focus {
  outline: none;
}
</style>