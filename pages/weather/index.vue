<template>
  <div id="page-index">
    <div id="main" style="width: 600px; height: 400px"></div>
  </div>
</template>

<script>
export default {
  head: {
    script: [
      {
        src: 'https://cdn.bootcdn.net/ajax/libs/echarts/5.1.2/echarts.common.js',
      },
    ],
  },
  mounted() {
    let weather = function () {
      fetch(
        'https://restapi.amap.com/v3/weather/weatherInfo?city=610112&key=99df86a54e3310465646f4a958c9d1fd&extensions=all',
        {
          method: 'get',
        }
      ).then((res) => {
        res.json().then((data) => {
          console.log(data)
          //   console.log(data.forecasts[0].casts[0].daytemp)
          let max = []
          let sin = []
          let week = []
          //   let weekall = data.forecasts[0].casts[0].date;
          let casts = data.forecasts[0].casts
          for (let i = 0; i < casts.length; i++) {
            let e = casts[i]
            // console.log("🌊", data);
            // console.log("🌊", e.daytemp);
            // console.log("🌊", e.nighttemp);
            max.push(e.daytemp)
            sin.push(e.nighttemp)
            week.push(e.date)
          }
          option.series[0].data = max
          option.series[1].data = sin
          option.xAxis.data = week
          myChart.setOption(option)
        })
      })
    }
    setInterval(() => {
      weather()
    }, 1000)
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'))
    // 指定图表的配置项和数据
    let option = {
      title: {
        text: '未来三天气温变化',
        subtext: '陕西西安',
      },
      tooltip: {
        trigger: 'axis',
      },
      legend: {
        data: ['最高气温', '最低气温'],
      },
      toolbox: {
        show: true,
        feature: {
          dataZoom: {
            yAxisIndex: 'none',
          },
          dataView: { readOnly: false },
          magicType: { type: ['line', 'bar'] },
          restore: {},
          saveAsImage: {},
        },
      },
      xAxis: {
        type: 'category',
        boundaryGap: false,
        data: ['今天', '明天', '后天', '大后天'],
      },
      yAxis: {
        type: 'value',
        axisLabel: {
          formatter: '{value} °C',
        },
      },
      series: [
        {
          name: '最高气温',
          type: 'line',
          data: [27, 30, 29, 30],
          markPoint: {
            data: [
              { type: 'max', name: '最大值' },
              { type: 'min', name: '最小值' },
            ],
          },
          markLine: {
            data: [{ type: 'average', name: '平均值' }],
          },
        },
        {
          name: '最低气温',
          type: 'line',
          data: [17, 21, 22, 19],
          markPoint: {
            data: [{ name: '周最低', value: -2, xAxis: 1, yAxis: -1.5 }],
          },
          markLine: {
            data: [
              { type: 'average', name: '平均值' },
              [
                {
                  symbol: 'none',
                  x: '90%',
                  yAxis: 'max',
                },
                {
                  symbol: 'circle',
                  label: {
                    position: 'start',
                    formatter: '最大值',
                  },
                  type: 'max',
                  name: '最高点',
                },
              ],
            ],
          },
        },
      ],
    }
    console.log('🌊', option)
  },
}
</script>
<style lang="stylus">
#page-index {
  #main {
    color: red;
  }
}
</style>