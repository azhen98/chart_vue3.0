<template>
  <div class="home">
    <div class="header">
      <div class="left">阿臻的手笔</div>
      <h2>全国疫情数据</h2>
      <div class="right">{{state.currentTimer}}</div>
    </div>
    <div class="main_box">
      <!-- 左边图表 -->
      <div class="column left">
        <div class="panle">
          <h2>折线图-就医情况</h2>
          <div class="panle_right"></div>
          <div class="chart" id="main_line"></div>
        </div>
        <div class="panle">
          <h2>柱状图-就医情况</h2>
          <div class="panle_right"></div>
          <div class="chart" id="main_column"></div>
        </div>
        <div class="panle">
          <h2>就医情况</h2>
          <div class="chart"></div>
          <div class="panle_right"></div>
        </div>
      </div>
      <!-- 中间图表 -->
      <div class="column">
        <!-- 数字展示模块 -->
        <div class="num">
          <div class="header">
            <h2>333</h2>
            <h2>3333</h2>
          </div>
          <div class="footer">
            <span>外国就医人数</span>
            <span>国内就医人数</span>
          </div>
        </div>
        <div class="map">
          <div class="map3"></div>
          <div class="map1"></div>
          <div class="map2"></div>
          <div id="main" class="column">2</div>
        </div>
      </div>
      <!-- 右侧图表 -->
      <div class="column">
        <div class="panle">
          <h2>就医情况</h2>
          <div class="chart"></div>
          <div class="panle_right"></div>
        </div>
        <div class="panle right">
          <h2>就医情况</h2>
          <div class="chart"></div>
          <div class="panle_right"></div>
        </div>
        <div class="panle">
          <h2>就医情况</h2>
          <div class="chart"></div>
          <div class="panle_right"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import "../utils/china"
// 引入 ECharts 主模块
import echarts from 'echarts/lib/echarts';

// 引入柱状图
import 'echarts/lib/chart/bar';
// 引入提示框和标题组件
import 'echarts/lib/component/tooltip';
import 'echarts/lib/component/title';
import "echarts/lib/chart/gauge";
import HelloWorld from '@/components/HelloWorld.vue'
import { ref, watch, reactive, computed, getCurrentInstance, watchEffect, onMounted, onUpdated, onUnmounted, onBeforeMount } from 'vue'
console.log(ref)
export default {
  name: 'Home',
  components: {
    HelloWorld
  },

  setup () {
    const state = reactive({ myChart: {}, currentTimer: "111111", myChart_line: {}, myChart_column: {} });
    function init () {
      /* 地图  */
      state.myChart = echarts.init(document.getElementById('main'))
      /* 折线图表 */
      state.myChart_line = echarts.init(document.getElementById('main_line'))
      /* 柱状图 */
      state.myChart_column = echarts.init(document.getElementById('main_column'))
      /* 配置项 */
      let option = {
        color: ['#3398DB'],
        tooltip: {
          trigger: 'axis',
          axisPointer: {            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '0',
          top: "10px",
          right: '0',
          bottom: '0',
          containLabel: true
        },
        xAxis: [
          {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
            axisTick: {
              alignWithLabel: true
            },
            axisLine: {
              show: false
            },
          },

        ],
        yAxis: [
          {
            type: 'value',
            axisLine: {
              lineStyle: {
                color: 'rgba(255,255,255,.1)'
              }
            },
            axisLabel: {
              fontSize: 12
            },
            splitLine: {
              color: 'rgba(255,255,255,1)'
            },

          }
        ],
        series: [
          {
            name: '直接访问',
            type: 'bar',
            barWidth: '35%',
            data: [10, 52, 200, 334, 390, 330, 220],
            itemStyle: {
              barBorderRadius: [2, 2, 0, 0]
            }
          }
        ],

      };
      /* 配置项 */
      let colors = ['#5793f3', '#d14a61', '#675bba'];
      let option2 = {

        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },

        grid: {
          left: '25%',
          right: '0',
          bottom: '0',
          top:"0",
          containLabel: false
        },
        xAxis: {
          show: false
        },
        yAxis: {
          type: 'category',
          data: ['巴西', '印尼', '美国', '印度', '中国', '世界人口(万)'],
          axisLine: {
            show: false
          },
          axisTick: {
            show: false
          },
          axisLabel: {
            color: "#fff"
          }
        },
        series: [
          {
            name: '2011年',
            type: 'bar',
            data: [18203, 23489, 29034, 104970, 131744, 630230],
            itemStyle: {
              barBorderRadius: 20,
            },
            barGategoryGap: 60,
            barWidth: 10
          },
          {
            name: '2012年',
            type: 'bar',
            data: [19325, 23438, 31000, 121594, 134141, 681807]
          }
        ]
      };


      state.myChart_line.setOption(option)
      state.myChart_column.setOption(option2)
      /* 地图配置项 */
      state.myChart.setOption({        series: [{
          type: 'map',
          map: 'china',
          name: "地图"
        }]      });

    }
    /* 组件挂载生命周期 */
    onMounted(() => {
      init()
      /* 实时显示当前时间 */
      const timer = setInterval(() => {
        state.currentTimer = moment().format('YYYY-MM-DD HH:mm:ss')
        console.log(state.currentTimer)
      }, 1000)
    })
    /* 数据return */
    return {
      state,
      init
    }
  },

}
</script>
<style lang="scss">
.home {
  // height: 100%;
  background: url('~@/assets/image/bg.jpg') no-repeat left top / 100% 100%;
  & > .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    height: 100px;
    color: #fff;
    background: url('~@/assets/image/head_bg.png') no-repeat left top / 100%
      auto;

    h2 {
      text-align: center;
      font-size: 38px;
      color: #fff;
      line-height: 80px;
    }
    .left,
    .right {
      text-align: center;
      width: 200px;
      line-height: 80px;
      color: rgba($color: #fff, $alpha: 0.5);
    }
  }
}
.main_box {
  display: flex;
  justify-content: space-between;
  .column {
    flex: 3;
    padding: 10px 20px 0;
    // background-color: #fff;
    .panle {
      height: 280px;
      padding: 0 15px 40px;
      background: url('../assets/image/line(1).png');
      margin-bottom: 20px;
      border: 1px solid rgba(25, 186, 139, 0.17);
      position: relative;
      box-sizing: border-box;
      &::before {
        position: absolute;
        left: -1px;
        top: -1px;
        width: 10px;
        height: 10px;
        color: #fff;
        border-left: 2px solid #02a6b5;
        border-top: 2px solid #02a6b5;
        content: '';
      }
      &::after {
        position: absolute;
        bottom: -1px;
        left: -1px;
        width: 10px;
        height: 10px;
        color: #fff;
        border-bottom: 2px solid #02a6b5;
        border-left: 2px solid #02a6b5;
        content: '';
      }
      .panle_right {
        position: absolute;
        bottom: 0;
        right: 0;
        width: 100%;
        height: 100%;
        &::after {
          position: absolute;
          top: -1px;
          right: -1px;
          width: 10px;
          height: 10px;
          color: #fff;
          border-top: 2px solid #02a6b5;
          border-right: 2px solid #02a6b5;
          content: '';
        }
        &::before {
          position: absolute;
          bottom: -1px;
          right: -1px;
          width: 10px;
          height: 10px;
          color: #fff;
          border-bottom: 2px solid #02a6b5;
          border-right: 2px solid #02a6b5;
          content: '';
        }
      }
      h2 {
        font-size: 16px;
        line-height: 1.8;
        color: #fff;
        text-align: center;
      }
      .chart {
        width: 100%;
        height: 250px;
        // background: #02a6b5;
      }
    }

    &:nth-child(2) {
      flex: 5;
    }
    .num {
      background-color: rgba(101, 132, 226, 0.1);
      padding: 10px 10px 5px;
      .header {
        position: relative;

        display: flex;
        justify-content: space-around;
        border: 1px solid rgba(25, 186, 139, 0.17);
        background: url('../assets/image/line(1).png') no-repeat center/100%
          100%;
        h2 {
          font-size: 32px;
          font-weight: bolder;
          color: sandybrown;
          line-height: 1.5;
          font-family: 'electronicFont';
        }
        &::before {
          position: absolute;
          left: -1px;
          top: -1px;
          width: 10px;
          height: 10px;
          color: #fff;
          border-left: 2px solid #02a6b5;
          border-top: 2px solid #02a6b5;
          content: '';
        }
        &::after {
          position: absolute;
          bottom: -1px;
          right: -1px;
          width: 10px;
          height: 10px;
          color: #fff;
          border-bottom: 2px solid #02a6b5;
          border-right: 2px solid #02a6b5;
          content: '';
        }
      }

      .footer {
        display: flex;
        justify-content: space-around;

        span {
          line-height: 2;
          font-size: 14px;
          color: #f5f5f5;
        }
      }
    }
    .map {
      overflow: hidden;
      position: relative;
      height: 900px;
    }
    .map1 {
      position: absolute;
      width: 650px;
      height: 650px;
      left: 0;
      right: 0;
      margin: 100px auto;
      background: url('../assets/image/map.png') no-repeat center/ 100% 100%;
    }
    .map2 {
      position: absolute;
      width: 800px;
      height: 800px;
      left: 0;
      right: 0;
      margin: 30px auto;
      background: url('../assets/image/jt.png') no-repeat center/ 100% 100%;
      animation: rotate2 15s linear infinite;
    }
    .map3 {
      position: absolute;
      width: 800px;
      height: 800px;
      left: 0;
      right: 0;
      margin: 30px auto;
      background: url('../assets/image/lbx.png') no-repeat center/ 100% 100%;
      animation: rotate1 15s linear infinite;
    }
    .map::after {
      display: block;
      content: '';
      clear: both;
    }
    @keyframes rotate2 {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(-360deg);
      }
    }
    @keyframes rotate1 {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(360deg);
      }
    }
  }
}

#main {
  width: 80%;
  height: 95%;
  margin: 0 auto;
}
#main_line,
#main_column {
  height: 100%;
}
</style>
