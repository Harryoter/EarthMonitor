<template>
  <div ref="echarts"></div>
</template>

<script>
import echarts from 'echarts'
export default {
  name: 'Echarts',
  props: {
    option: {
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {
      echarts: null
    }
  },
  mounted() {
    this.initChart()
  },
  destroyed() {
    this.clearChart()
  },
  methods: {
    initChart() {
      this.echarts = echarts.init(this.$refs.echarts)
      if (this.option.geo) {
        echarts.registerMap(this.option.geo.map, require('../lib/' + this.option.geo.map + '.json'))
        this.echarts.on('click', params => this.$emit('handleMapScatter', { params }))
        this.echarts.on('legendselectchanged', params => this.$emit('handleMapLegend', { params }))
        this.echarts.on('georoam', params => this.$emit('handleMapGeoroam', { params, option: this.echarts.getOption().geo }))
      }
      this.echarts.setOption(this.option)
    },
    clearChart() {
      this.echarts.clear()
      this.echarts.off('click')
      this.echarts.off('legendselectchanged')
    }
  }
}
</script>
<style scoped lang="scss">
</style>