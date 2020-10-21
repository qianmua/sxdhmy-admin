<template>
  <div class="mod-demo-echarts">
    <el-alert
      title="提示："
      type="warning"
      :closable="false">
      <div >
        <!-- <p class="el-alert__description">1. 此Demo只提供ECharts官方使用文档，入门部署和体验功能。具体使用请参考：http://echarts.baidu.com/index.html</p> -->
        <p class="el-alert__description">数据可能存在延时，请勿重复刷新</p>
      </div>
    </el-alert>

    <el-row :gutter="20">
      <el-col :span="24">
        <el-card>
          <div id="J_chartLineBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="24">
        <el-card>
          <div id="J_chartBarBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartPieBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card>
          <div id="J_chartScatterBox" class="chart-box"></div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    data () {
      return {
        chartLine: null,
        chartBar: null,
        chartPie: null,
        chartScatter: null
      }
    },
    mounted () {
      this.initChartLine()
      this.initChartBar()
      this.initChartPie()
      // this.initChartScatter()
    },
    activated () {
      // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
      if (this.chartLine) {
        this.chartLine.resize()
      }
      if (this.chartBar) {
        this.chartBar.resize()
      }
      if (this.chartPie) {
        this.chartPie.resize()
      }
      if (this.chartScatter) {
        this.chartScatter.resize()
      }
    },
    methods: {
      // 折线图
      initChartLine () {
        var option = {
          'title': {
            'text': '平台活跃用户数量'
          },
          'tooltip': {
            'trigger': 'axis'
          },
          'legend': {
            'data': [ '管理员活跃', '首页资源', '直接访问', '合作工厂', '搜索引擎' ]
          },
          'grid': {
            'left': '3%',
            'right': '4%',
            'bottom': '3%',
            'containLabel': true
          },
          'toolbox': {
            'feature': {
              'saveAsImage': { }
            }
          },
          'xAxis': {
            'type': 'category',
            'boundaryGap': false,
            'data': [ '四月', '五月', '六月', '七月', '八月', '九月', '本月' ]
          },
          'yAxis': {
            'type': 'value'
          },
          'series': [
            {
              'name': '管理员活跃',
              'type': 'line',
              'stack': '总量',
              'data': [ 4, 12, 7, 6, 8, 20, 19 ]
            },
            {
              'name': '首页资源',
              'type': 'line',
              'stack': '总量',
              'data': [ 5, 15, 15, 23, 21, 31, 33 ]
            },
            {
              'name': '直接访问',
              'type': 'line',
              'stack': '总量',
              'data': [ 8, 13, 32, 44, 55, 31, 21 ]
            },
            {
              'name': '合作工厂',
              'type': 'line',
              'stack': '总量',
              'data': [ 11, 43, 53, 67, 88, 99, 77 ]
            },
            {
              'name': '搜索引擎',
              'type': 'line',
              'stack': '总量',
              'data': [ 44, 21, 18, 19, 20, 31, 28 ]
            }
          ]
        }
        this.chartLine = echarts.init(document.getElementById('J_chartLineBox'))
        this.chartLine.setOption(option)
        window.addEventListener('resize', () => {
          this.chartLine.resize()
        })
      },
      // 柱状图
      initChartBar () {
        var option = {
          'title': {
            'text': '月出合同统计'
          },
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            }
          },
          legend: {
            data: ['浩丰', '福安', '康达', '虹彩', '喜福来', '宏艺', '汇越', '泰宇', '天顺']
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: [
            {
              type: 'category',
              data: ['四月', '五月', '六月', '七月', '八月', '九月', '本月']
            }
          ],
          yAxis: [
            {
              type: 'value'
            }
          ],
          series: [
            {
              name: '浩丰',
              type: 'bar',
              data: [1, 2, 3, 1, 5, 2, 3]
            },
            {
              name: '福安',
              type: 'bar',
              stack: '广告',
              data: [2, 5, 3, 2, 1, 3, 2]
            },
            {
              name: '康达',
              type: 'bar',
              stack: '广告',
              data: [4, 3, 2, 5, 2, 1, 5]
            },
            {
              name: '虹彩',
              type: 'bar',
              stack: '广告',
              data: [8, 1, 3, 2, 3, 4, 7]
            },
            {
              name: '喜福来',
              type: 'bar',
              data: [8, 7, 1, 2, 4, 5, 6],
              markLine: {
                lineStyle: {
                  normal: {
                    type: 'dashed'
                  }
                },
                data: [
                  [{ type: 'min' }, { type: 'max' }]
                ]
              }
            },
            {
              name: '宏艺',
              type: 'bar',
              barWidth: 5,
              stack: '宏艺',
              data: [5, 4, 3, 5, 5, 3, 1]
            },
            {
              name: '汇越',
              type: 'bar',
              stack: '搜索引擎',
              data: [2, 3, 3, 4, 5, 3, 1]
            },
            {
              name: '泰宇',
              type: 'bar',
              stack: '搜索引擎',
              data: [2, 3, 4, 5, 6, 3, 4]
            },
            {
              name: '天顺',
              type: 'bar',
              stack: '搜索引擎',
              data: [5, 6, 1, 2, 3, 1, 4]
            }
          ]
        }
        this.chartBar = echarts.init(document.getElementById('J_chartBarBox'))
        this.chartBar.setOption(option)
        window.addEventListener('resize', () => {
          this.chartBar.resize()
        })
      },
      // 饼状图
      initChartPie () {
        var option = {
          backgroundColor: '#2c343c',
          title: {
            text: 'Customized Pie',
            left: 'center',
            top: 20,
            textStyle: {
              color: '#ccc'
            }
          },
          tooltip: {
            trigger: 'item',
            formatter: '{a} <br/>{b} : {c} ({d}%)'
          },
          visualMap: {
            show: false,
            min: 80,
            max: 600,
            inRange: {
              colorLightness: [0, 1]
            }
          },
          series: [
            {
              name: '访问来源',
              type: 'pie',
              radius: '55%',
              center: ['50%', '50%'],
              data: [
                { value: 335, name: 'A' },
                { value: 310, name: 'B' },
                { value: 274, name: 'C' },
                { value: 235, name: 'D' },
                { value: 400, name: 'E' }
              ].sort(function (a, b) { return a.value - b.value }),
              roseType: 'radius',
              label: {
                normal: {
                  textStyle: {
                    color: 'rgba(255, 255, 255, 0.3)'
                  }
                }
              },
              labelLine: {
                normal: {
                  lineStyle: {
                    color: 'rgba(255, 255, 255, 0.3)'
                  },
                  smooth: 0.2,
                  length: 10,
                  length2: 20
                }
              },
              itemStyle: {
                normal: {
                  color: '#c23531',
                  shadowBlur: 200,
                  shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
              },
              animationType: 'scale',
              animationEasing: 'elasticOut',
              animationDelay: function (idx) {
                return Math.random() * 200
              }
            }
          ]
        }
        this.chartPie = echarts.init(document.getElementById('J_chartPieBox'))
        this.chartPie.setOption(option)
        window.addEventListener('resize', () => {
          this.chartPie.resize()
        })
      },
      // 散点图
      // initChartScatter () {
      //   var option = {
      //     backgroundColor: new echarts.graphic.RadialGradient(0.3, 0.3, 0.8, [
      //       { offset: 0, color: '#f7f8fa' },
      //       { offset: 1, color: '#cdd0d5' }
      //     ]),
      //     title: {
      //       text: '1990 与 2015 年各国家人均寿命与 GDP'
      //     },
      //     legend: {
      //       right: 10,
      //       data: ['1990', '2015']
      //     },
      //     xAxis: {
      //       splitLine: {
      //         lineStyle: {
      //           type: 'dashed'
      //         }
      //       }
      //     },
      //     yAxis: {
      //       splitLine: {
      //         lineStyle: {
      //           type: 'dashed'
      //         }
      //       },
      //       scale: true
      //     },
      //     series: [
      //       {
      //         name: '1990',
      //         data: [
      //           [28604, 77, 17096869, 'Australia', 1990],
      //           [31163, 77.4, 27662440, 'Canada', 1990],
      //           [1516, 68, 1154605773, 'China', 1990],
      //           [13670, 74.7, 10582082, 'Cuba', 1990],
      //           [28599, 75, 4986705, 'Finland', 1990],
      //           [29476, 77.1, 56943299, 'France', 1990],
      //           [31476, 75.4, 78958237, 'Germany', 1990],
      //           [28666, 78.1, 254830, 'Iceland', 1990],
      //           [1777, 57.7, 870601776, 'India', 1990],
      //           [29550, 79.1, 122249285, 'Japan', 1990],
      //           [2076, 67.9, 20194354, 'North Korea', 1990],
      //           [12087, 72, 42972254, 'South Korea', 1990],
      //           [24021, 75.4, 3397534, 'New Zealand', 1990],
      //           [43296, 76.8, 4240375, 'Norway', 1990],
      //           [10088, 70.8, 38195258, 'Poland', 1990],
      //           [19349, 69.6, 147568552, 'Russia', 1990],
      //           [10670, 67.3, 53994605, 'Turkey', 1990],
      //           [26424, 75.7, 57110117, 'United Kingdom', 1990],
      //           [37062, 75.4, 252847810, 'United States', 1990]
      //         ],
      //         type: 'scatter',
      //         symbolSize: function (data) {
      //           return Math.sqrt(data[2]) / 5e2
      //         },
      //         label: {
      //           emphasis: {
      //             show: true,
      //             formatter: function (param) {
      //               return param.data[3]
      //             },
      //             position: 'top'
      //           }
      //         },
      //         itemStyle: {
      //           normal: {
      //             shadowBlur: 10,
      //             shadowColor: 'rgba(120, 36, 50, 0.5)',
      //             shadowOffsetY: 5,
      //             color: new echarts.graphic.RadialGradient(0.4, 0.3, 1, [
      //               { offset: 0, color: 'rgb(251, 118, 123)' },
      //               { offset: 1, color: 'rgb(204, 46, 72)' }
      //             ])
      //           }
      //         }
      //       },
      //       {
      //         name: '2015',
      //         data: [
      //           [44056, 81.8, 23968973, 'Australia', 2015],
      //           [43294, 81.7, 35939927, 'Canada', 2015],
      //           [13334, 76.9, 1376048943, 'China', 2015],
      //           [21291, 78.5, 11389562, 'Cuba', 2015],
      //           [38923, 80.8, 5503457, 'Finland', 2015],
      //           [37599, 81.9, 64395345, 'France', 2015],
      //           [44053, 81.1, 80688545, 'Germany', 2015],
      //           [42182, 82.8, 329425, 'Iceland', 2015],
      //           [5903, 66.8, 1311050527, 'India', 2015],
      //           [36162, 83.5, 126573481, 'Japan', 2015],
      //           [1390, 71.4, 25155317, 'North Korea', 2015],
      //           [34644, 80.7, 50293439, 'South Korea', 2015],
      //           [34186, 80.6, 4528526, 'New Zealand', 2015],
      //           [64304, 81.6, 5210967, 'Norway', 2015],
      //           [24787, 77.3, 38611794, 'Poland', 2015],
      //           [23038, 73.13, 143456918, 'Russia', 2015],
      //           [19360, 76.5, 78665830, 'Turkey', 2015],
      //           [38225, 81.4, 64715810, 'United Kingdom', 2015],
      //           [53354, 79.1, 321773631, 'United States', 2015]
      //         ],
      //         type: 'scatter',
      //         symbolSize: function (data) {
      //           return Math.sqrt(data[2]) / 5e2
      //         },
      //         label: {
      //           emphasis: {
      //             show: true,
      //             formatter: function (param) {
      //               return param.data[3]
      //             },
      //             position: 'top'
      //           }
      //         },
      //         itemStyle: {
      //           normal: {
      //             shadowBlur: 10,
      //             shadowColor: 'rgba(25, 100, 150, 0.5)',
      //             shadowOffsetY: 5,
      //             color: new echarts.graphic.RadialGradient(0.4, 0.3, 1, [
      //               { offset: 0, color: 'rgb(129, 227, 238)' },
      //               { offset: 1, color: 'rgb(25, 183, 207)' }
      //             ])
      //           }
      //         }
      //       }
      //     ]
      //   }
      //   this.chartPie = echarts.init(document.getElementById('J_chartScatterBox'))
      //   this.chartPie.setOption(option)
      //   window.addEventListener('resize', () => {
      //     this.chartPie.resize()
      //   })
      // }
    }
  }
</script>

<style lang="scss">
  .mod-demo-echarts {
    > .el-alert {
      margin-bottom: 10px;
    }
    > .el-row {
      margin-top: -10px;
      margin-bottom: -10px;
      .el-col {
        padding-top: 10px;
        padding-bottom: 10px;
      }
    }
    .chart-box {
      min-height: 400px;
    }
  }
</style>
