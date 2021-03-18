<template>
  <div id="rose-chart">
    <div class="rose-chart-title">APP 日活统计</div>
    <!--<dv-charts :option="option" />-->
    <div id="echars-dau" :style="{width: '100%', height: '100%'}"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import axios from 'axios'

export default {
  name: 'RoseChart',
  data () {
    return {
      chart: null,
      option: Object,
      count: 11
    }
  },
  created () {
    this.option = {
      title: {
        text: ''
      },
      tooltip: {
        trigger: 'axis'
      },
      legend: {
        data: ['昨日', '今日'],
        left: 40,
        textStyle: {
          color: '#FFFFFF'
        }
      },
      toolbox: {
        show: true,
        feature: {
          dataZoom: {
            yAxisIndex: 'none'
          },
          dataView: { readOnly: false },
          magicType: { type: ['line', 'bar'] },
          restore: {},
          saveAsImage: {}
        },
        right: 40,
        iconStyle: {
          borderColor: '#FFFFFF'
        }
      },
      xAxis: {
        type: 'category',
        boundaryGap: false,
        data: [1, 2, 3, 4, 5],
        axisLine: {
          lineStyle: {
            color: 'RGB(0, 184, 239)'
          }
        },
        axisLabel: {
          color: '#FFF'
        }
      },
      yAxis: {
        type: 'value',
        axisLabel: {
          formatter: '{value} 万',
          color: '#FFF'
        },
        axisLine: {
          lineStyle: {
            color: 'RGB(0, 184, 239)'
          }
        }
      },
      series: [
        {
          name: '昨日',
          type: 'line',
          data: [1, 2, 3, 4, 5],
          markPoint: {
            data: [
              { type: 'max', name: '最大值' },
              { type: 'min', name: '最小值' }
            ]
          },
          markLine: {
            data: [
              [{
                symbol: 'none',
                x: '90%',
                yAxis: 'max'
              }, {
                symbol: 'circle',
                label: {
                  position: 'start',
                  formatter: '最大值',
                  color: '#FFF'
                },
                type: 'max',
                name: '最高点'
              }]
            ]
          }
        },
        {
          name: '今日',
          type: 'line',
          data: [5, 6, 7, 8, 9]
        }
      ]
    }
  },
  methods: {
    initEcharts () {
      this.chart = echarts.init(document.getElementById('echars-dau'))
      this.getData()
      this.chart.setOption(this.option)
      setInterval(() => {
        this.getData()
        this.chart.setOption(this.option)
      }, 2000)
    },
    getData () {
      axios.get('http://localhost:8087/dau/test').then((res) => {
        this.option.series[0].data = res.data.data.yesCount
        this.option.xAxis.data = res.data.data.dh
        this.option.series[1].data = res.data.data.dayCount
      }).catch(function (error) {
        console.log(error)
      })
    }
  },
  mounted () {
    this.initEcharts()
  }
}
</script>

<style lang="less">
#rose-chart {
  width: 50%;
  height: 100%;
  background-color: rgba(6, 30, 93, 0.5);
  border-top: 2px solid rgba(1, 153, 209, .5);
  box-sizing: border-box;

  .rose-chart-title {
    height: 50px;
    font-weight: bold;
    text-indent: 20px;
    font-size: 20px;
    display: block;
    text-align: center;
  }

  .dv-charts-container {
    height: calc(~"100% - 50px");
  }
}
</style>
