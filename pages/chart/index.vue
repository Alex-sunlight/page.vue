<template>
  <div class="all" id="box">
    <div class="screen" id="screen">
      <ul>
        <li>
          <img src="~/assets/chart/111.jpg" width="500" height="300" />
        </li>
        <li>
          <img src="~/assets/chart/222.jpg" width="500" height="300" />
        </li>
        <li>
          <img src="~/assets/chart/333.png" width="500" height="300" />
        </li>
        <li>
          <img src="~/assets/chart/444.jpg" width="500" height="300" />
        </li>
        <li>
          <img src="~/assets/chart/555.jpeg" width="500" height="300" />
        </li>
      </ul>
      <ol></ol>
    </div>
    <div id="arr">
      <span id="left">&lt;</span>
      <span id="right">&gt;</span>
    </div>
  </div>
</template>
<script>
export default {
  mounted() {
    function animate(element, target, step, callback) {
      //    清除定时器
      if (element.timeid) {
        clearInterval(element.timeid)
      } //    1. 设置定时器
      element.timeid = setInterval(function () {
        //        2. 获取元素的当前位置
        var current = element.offsetLeft
        //        3. 根据当前位置加上/减去我们的步进
        //         如果当前位置大于目标位置,就是减去步进.否则就是加上步进
        if (current > target) {
          //            证明是从右往左,应该减去step
          var pos = current - step
        } else {
          // 证明是从左往右,应该加上step
          var pos = current + step
        }

        //          4. 给element赋值新的位置
        element.style.left = pos + 'px'

        //          5. 判断是否到达目标位置,如果到达目标位置就停下来
        //          5.1 如何判断已经到达目标位置
        //              如果 当前位置(赋值之后的位置 pos)- 目标位置 的差值绝对值,小于步进,证明马上要到目标位置了
        if (Math.abs(pos - target) <= step) {
          //证明马上就要到达目标位置了
          //              5.2  清除定时器,直接把目标位置给元素
          clearInterval(element.timeid)
          element.style.left = target + 'px'
          //判断用户有没有传第四个参数,如果传了就调用,如果没传就不调用
          //        如果传入了函数,callback转换成布尔就是true,如果什么都没传,callback转换成布尔,就是false
          if (callback) {
            callback()
          }
        }
      }, 15)
    }
    var screen = document.getElementById('screen')
    var ul = screen.getElementsByTagName('ul')[0]
    var lis = ul.children
    var ol = screen.getElementsByTagName('ol')[0]
    var indexGlobal = 0 //用于存储当前展示的是哪张图片的下标

    //          1.2 遍历ul里面的li
    for (var i = 0; i < lis.length; i++) {
      // 1.3 在遍历的过程中,动态的创建ol中的li,然后把新创建的li放到ol中
      var li = document.createElement('li')
      //由于点击的时候,需要获取当前li的下标,所以在创建的时候就存储一下
      li.setAttribute('index', i)
      // 1.4 给每一个数字按钮,添加数字
      li.innerText = i + 1
      //1.5 默认一打开,展示第一个图片,所以要给数字按钮1默认高亮
      if (i == 0) {
        //                    li.style.backgroundColor = 'yellow';
        li.className = 'current'
      }
      ol.appendChild(li)

      //       2. 点击数字按钮,让轮播图滚动(修改ul的left).
      //        2.1 获取元素 ol里面的li
      //        2.2 给每一个ol中的li注册点击事件
      li.onclick = fn
    }

    //要实现无缝轮播的效果,需要克隆第一张图放到最后面
    //  注意:一定要在数字按钮,创建完毕之后在克隆
    var newone = lis[0].cloneNode(true)
    ul.appendChild(newone)
    console.log(lis)

    //        数字按钮li的事件处理函数
    function fn() {
      //  2.3 在事件处理函数中,修改ul的left
      //            2.3.1 目标值是多少?
      //                目标值 = 当前图片的下标 * 图片的宽度
      //                目标值 = 数字按钮的下标(this.getAttribute('index') * screen的宽度(screen.offsetWidth)
      //            console.log(this);

      var target = this.getAttribute('index') * screen.offsetWidth
      //            2.3.2  将目标值赋值为ul, 注意值是负值
      //            ul.style.left = -target + 'px'; //直接到了目标位置
      //我们需要慢慢滚动过去
      animate(ul, -target, 20)
      //            2.4 点击哪个数字按钮,就让谁高亮
      //排他
      for (var i = 0; i < ol.children.length; i++) {
        ol.children[i].className = ''
      }
      this.className = 'current'

      //    /动态的修改indexGlobal的值,因为indexGlobal要时刻和展示的图片的下标保持一致
      indexGlobal = this.getAttribute('index')
      console.log(indexGlobal)
    }

    //3. 鼠标移入到box中,arr展示,鼠标移出到box外,arr消失
    //3.1 获取元素
    var box = document.getElementById('box')
    var arr = document.getElementById('arr')
    //        3.2 给box注册鼠标移入,移出事件
    box.onmouseenter = function () {
      //        3.3 移入事件,arr展示
      arr.style.display = 'block'

      // 7.1 鼠标移入停止自动轮播
      clearInterval(timeid)
    }
    box.onmouseleave = function () {
      //        3.5 移出事件,arr消失
      arr.style.display = 'none'

      //7.2 鼠标移出,开始自动轮播
      timeid = setInterval(function () {
        right.onclick()
      }, 3000) //注意: 自动轮播的时间不能太短,因为图片滚动也需要时间
    }

    //4. 点击右边的arr,滚动到下一张图
    //        4.1 获取元素
    var right = document.getElementById('right')
    //        4.2 给right注册点击事件
    right.onclick = function () {
      //        4.3 在事件处理函数中,滚动到下一张图
      //    4.3.1 当前是哪一张? 通过indexGlobal获取到当前的下标
      // 4.4 判断当前是否是第5张图,如果是第5张图,就需要自己写代码,慢慢滚动到第6张
      if (indexGlobal == ol.children.length - 1) {
        //证明是第5张,需要自己写代码,滚动过去,实现高亮
        //      4.4.1 滚动到第6张
        indexGlobal++
        //计算展示第6张图,ul应该处于的位置
        var target = indexGlobal * screen.offsetWidth
        animate(ul, -target, 20, function () {
          //4.4.2 滚动到第6张之后,要立刻跳到第一张
          ul.style.left = '0px'
          //indexGlobal应该和展示的图片保持一致
          indexGlobal = 0
        })

        //      4.4.3 实现高亮
        //排他
        for (var i = 0; i < ol.children.length; i++) {
          ol.children[i].className = ''
        }
        ol.children[0].className = 'current'
      } else {
        //    4.3.2 根据当前找到下一个数字按钮,触发数字按钮的点击事件
        indexGlobal++
        //    console.log(ol.children[indexGlobal]);
        ol.children[indexGlobal].onclick()
      }
    }

    // 5. 点击左边的arr,滚动到上一张图
    //  5.1 获取元素
    var left = document.getElementById('left')
    //  5.2 给left注册点击事件
    left.onclick = function () {
      //  5.3 在事件处理函数中,根据当前的图片,展示上一张
      //5.3.1 判断当前展示的是不是第一张,如果是第一张,直接跳到最后一张,然后滚动到第5张
      if (indexGlobal == 0) {
        //直接跳到最后一张
        indexGlobal = ol.children.length
        ul.style.left = -indexGlobal * screen.offsetWidth + 'px'
        //然后再滚动到第五张
        indexGlobal--
        ol.children[indexGlobal].onclick()
      } else {
        indexGlobal--
        ol.children[indexGlobal].onclick()
      }
    }

    // 6. 让轮播图自动轮播
    var timeid = setInterval(function () {
      right.onclick()
    }, 3000)
  },
}
</script>
<style lang="stylus">
body {
  background: black;
}

* {
  padding: 0;
  margin: 0;
  list-style: none;
  border: 0;
}

.all {
  width: 500px;
  height: 300px;
  padding: 7px;
  border: 1px solid #ccc;
  margin: 100px auto;
  position: relative;
}

.screen {
  width: 500px;
  height: 300px;
  overflow: hidden;
  position: relative;
}

.screen li {
  width: 500px;
  height: 300px;
  overflow: hidden;
  float: left;
}

.screen ul {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 3000px;
}

.all ol {
  position: absolute;
  right: 10px;
  bottom: 10px;
  line-height: 20px;
  text-align: center;
}

.all ol li {
  float: left;
  width: 20px;
  height: 20px;
  background: #fff;
  border: 1px solid #ccc;
  margin-left: 10px;
  cursor: pointer;
}

.all ol li.current {
  background: yellow;
}

#arr {
  display: none;
  z-index: 1000;
}

#arr span {
  width: 40px;
  height: 40px;
  position: absolute;
  left: 5px;
  top: 50%;
  margin-top: -20px;
  background: #000;
  cursor: pointer;
  line-height: 40px;
  text-align: center;
  font-weight: bold;
  font-family: '黑体';
  font-size: 30px;
  color: #fff;
  opacity: 0.3;
  border: 1px solid #fff;
}

#arr #right {
  right: 5px;
  left: auto;
}
</style>

