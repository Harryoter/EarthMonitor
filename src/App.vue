<template>
  <div class="home">
    <div class="home-container">
      <header>地空协作灾害监测搜救系统</header>
      <div class="small-text">aaa</div>
      <div class="small-video"  :style="{ width: videoWidth }">
        <video-player ref="videoPlayer"></video-player>
      </div>
      <device-monitor :resourceList="resourceList"></device-monitor>
      <hello-potree></hello-potree>
      <data-view></data-view>
    </div>
  </div>
</template>

<script>
import HelloPotree from './components/HelloPotree'
import VideoPlayer from './components/VideoPlayer'
import DeviceMonitor from './components/DeviceMonitor'
import DataView from './components/DataView'
export default {
  name: 'App',
  components:{HelloPotree, VideoPlayer, DeviceMonitor, HelloPotree, DataView},
  data() {
    return {
      videoWidth: '480px',
      resourceList: [
        { name: '无人机', type: 1, img: 'two.png', active: false, data: [{name:'固定翼无人机1'},{name:'固定翼无人机2'},{name:'旋翼无人机1'},{name:'旋翼无人机2'}] },
        { name: '地面移动车', type: 2, img: 'three.png', active: false, data: [{name:'地面移动车1'},{name:'地面移动车2'}] }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  position: relative;
  height: 100vh;
  &-container {
    width: 1920px;
    height: 1080px;
    overflow: hidden;
    background: #000928;
    background-size: 100% 100%;
    position: relative;
    header {
      width: 1880px;
      height: 112px;
      background: url("./assets/images/header.png") no-repeat center;
      background-size: 100% 100%;
      position: relative;
      margin: 0 auto;
      z-index: 2;
      font-weight: normal;
      line-height: 112px;
      text-align: center;
      font-size: 26px;
      color: aqua;
    }
    .small-video{
      position: absolute;
      top: 140px;
      left: 10px;
      z-index: 2;
      border:1px solid #ccc;
      background:red;
      transition: all 0.3s;
      ::v-deep {
        .vjs-text-track-display {
          pointer-events: auto;
        }
      }
    }
    .small-text{
      position: absolute;
      top: 110px;
      left: 10px;
      z-index: 2;
      color: aqua;
    }
  }
}
</style>
