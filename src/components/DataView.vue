<template>
  <div class="data">
    <div class="data-list">
      <div class="data-list-title">救援情况统计</div>
      <div class="data-list-view">
        <echarts-view
          class="data-list-view-rescue"
          :option="rescueOption"
        ></echarts-view>
      </div>
    </div>
  </div>
</template>

<script>
import EchartsView from '@/components/EchartsView'
export default {
  name: 'DataView',
  components: { EchartsView },
  props: {
    weatherCity: {
      type: String
    }
  },
  data() {
    return {
      colorList: [['#3F28D0', '#0282DE'], ['#FC9501', '#FED701'], ['#FE411B', '#FE9B1A'], ['#551AFE', '#7B1BFE']],
      rescueOption: {
        tooltip :{
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          bottom: 10,
          left: 'center',
          data: []
        },
        series: [
          {
            type: 'pie',
            radius: '65%',
            center: ['50%', '50%'],
            selectedMode: 'single',
            data: []
          }
        ]
      },
      resourceOption: {
        xAxis: {
          data: [],
          axisLine: {
            show: false,
            fontSize: 13,
            lineStyle: { color: '#67E0E3' }
          },
          axisLabel: {
            interval: 0
          },
          axisTick: {
            show: false
          }
        },
        yAxis: {
          axisLine: {
            show: false,
            lineStyle: { color: '#67E0E3' }
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            fontSize: 13,
            formatter: '{value}'
          }
        },
        grid: {
          left: '20%'
        },
        series: [{
          data: [],
          type: 'bar',
          barWidth: 40,
          label: {
            show: true,
            position: 'top',
            color: '#ffffff',
            fontSize: 13,
            formatter: (serie) => {
              return `${serie.data}`
            }
          },
          itemStyle: {
            color: (params) => {
              return {
                type: 'linear',
                x: 0.5,
                y: 1,
                x2: 0.5,
                y2: 0,
                colorStops: [{
                  offset: 0, color: this.colorList[params.dataIndex][0]
                }, {
                  offset: 1, color: this.colorList[params.dataIndex][1]
                }]
              }
            }
          },
          animationDuration: 500,
          animationEasing: 'linear',
          animationDelay: (params) => {
            return params * 100
          },
          animationDurationUpdate: 500,
          animationEasingUpdate: 'linear'
        }]
      }
    }
  },
  created() {
    this.setRescueOption()
    this.setResourceOption()
  },
  methods: {
    setRescueOption() {
      this.rescueOption.legend.data = ['压埋人数', '已救人数', '救援中人数']
      this.rescueOption.series[0].data = [{ value: 40, name: '压埋人数'}, { value: 130, name: '已救人数'},{ value: 90, name: '救援中人数'}]
    },
    setResourceOption() {
      this.resourceOption.xAxis.data = ['固定翼无人机', '旋翼无人机', '地面移动车', '救援人员']
      this.resourceOption.series[0].data = [40, 130, 90, 100]
    }
  }
}
</script>
<style scoped lang="scss">
.data {
  width: 450px;
  position: absolute;
  top: 140px;
  right: 10px;
  z-index: 2;
  &-list {
    &-title {
      width: 100%;
      height: 49px;
      margin:0 0 5px 0;
      background: url("../assets/images/echartsTitle.png") no-repeat center;
      background-size: 100% 100%;
      line-height: 49px;
      color: aqua;
      text-indent: 25px;
      font-weight: bold;
      font-size: 15px;
      letter-spacing: 2px;
    }
    &-view {
      width: 100%;
      height: 290px;
      margin: 0 0 20px 0;
      background: url("../assets/images/echartsView.png") no-repeat center;
      background-size: 100% 100%;
      &-rescue,&-resource{
        position: relative;
        width: 100%;
        height: 100%;
      }
    }
  }
}
</style>
