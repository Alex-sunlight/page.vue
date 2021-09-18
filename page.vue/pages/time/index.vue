<template>
  <div id="day-time">
    <div class="time-gua">
      <div class="time">下午2:30</div>
      <div class="date">
        <span class="today">2021年8月20日</span>
        <span class="week">(星期五)</span>
        <span class="last">(GMT+8)</span>
      </div>

      <div class="china">中国北京时间</div>
      <br />
      <div class="xian"></div>
    </div>
  </div>
</template>
<script>
export default {
  head: {
    script: [
      {
        src: 'https://unpkg.com/dayjs@1.8.21/dayjs.min.js',
      },
    ],
  },
  mounted() {
    const time = function () {
      let A = dayjs().format('A')
      if (A === 'PM') {
        A = '下午'
      } else {
        A = '上午'
      }
      document.querySelector('.time').innerText = A + dayjs().format('h:m:s')
      document.querySelector('.today').innerText =
        dayjs().format(' YYYY 年 MM 月 DD 日 ')
    }
    setInterval(() => {
      time()
    }, 1000)
  },
}
</script>
<style lang="stylus">
#day-time {
  .time {
    color: #222222;
    font-size: 36px;
    margin-top: 20px;
    margin-bottom: 20px;
  }

  .date {
    margin-bottom: 10px;
  }

  .today {
    color: #70757a;
    margin: 0px 0px 2px;
    font-size: 16px;
  }

  .week {
    color: #70757a;
    font-size: 16px;
  }

  .last {
    color: #70757a;
    margin-left: 20;
    margin-right: 20;
    font-size: 16px;
  }

  .china {
    color: #70757a;
    margin: 0px 0px 2px;
    font-size: 16px;
  }

  .time-gua {
    margin-left: 90px;
  }

  .xian {
    width: 800px;
    height: 1px;
    background-color: #d5d5d5;
  }
}
</style>
