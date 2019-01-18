<template>
  <div class="desk-page">
    <head-nav :style="{height:screenfull_state?'160px':'100px'}"></head-nav>
    <div class="page-main">
      <div class="move-box" id="moveBox">
        <ul id="moveBoxWrapper" class="move-box__wrapper">
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button">
              <i class="iconfont">&#xe6d6;</i>
            </h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <line-urban-management v-show="!bar_chart_status" ref="_urban_management_line">
              <span>
                <i class="icon-class iconfont" @click="largeMethod('LineUrbanManagement')">&#xe655;</i>
              </span>
            </line-urban-management>
          </li>
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button"><i class="iconfont">&#xe6d6;</i></h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <river-quality>
              <span>
                <i class="icon-class iconfont" @click="largeMethod('RiverQuality')">&#xe655;</i>
              </span>
            </river-quality>
          </li>
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button"><i class="iconfont">&#xe6d6;</i></h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <screen-panel-title :style="{textAlign: 'left'}">雨量统计
              <i :style="{position: 'initial'}" class="icon-class iconfont" @click="largeMethod('LineUrbanManagement')">&#xe655;</i>
            </screen-panel-title>
            <bar-simple-chart :theme="'dark'" :style="{height: '314px', width: '480px', background: 'rgba(0, 255, 255, 0.06)'}" ref="_bar_simple_chart" />
          </li>
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button"><i class="iconfont">&#xe6d6;</i></h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <pie-riverway :style="{height: '360px'}">
              <span>
                <i class="icon-class iconfont" @click="largeMethod('PieRiverway')">&#xe655;</i>
              </span>
            </pie-riverway>
          </li>
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button"><i class="iconfont">&#xe6d6;</i></h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <screen-panel-title :style="{textAlign: 'left'}">闸泵站
              <i :style="{position: 'initial'}" class="icon-class iconfont" @click="largeMethod('LineUrbanManagement')">&#xe655;</i>
            </screen-panel-title>
            <div class="item-content">
              <sluice-pump-preview theme="dark" :stationId="1"></sluice-pump-preview>
            </div>
          </li>
          <li class="item-wrapper move-box__item">
            <h6 class="drap_button"><i class="iconfont">&#xe6d6;</i></h6>
            <div class="tool-tip">
              摁住拖拽可以调整布局
            </div>
            <screen-panel-title :style="{textAlign: 'left'}">卫星云图
              <i :style="{position: 'initial'}" class="icon-class iconfont" @click="largeMethod('LineUrbanManagement')">&#xe655;</i>
            </screen-panel-title>
            <div class="item-content">
              <img :src="weatherImgUrl" alt="">
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
/**
  * @name desk-page (组件名称)
  * @module 组件存放位置
  * @desc 组件描述
  * @author Jacke
  * @date
  * @param {Object} [title]    - 参数说明
  * @param {String} [columns] - 参数说明
  * @example 调用示例
  *  <desk-page></desk-page>
  */
import HeadNav from '@/components/HeadNav'
import LineUrbanManagement from '@/views/subject/components/LeftStatistical/components/LineUrbanManagement/index.vue'
import RiverQuality from '@/views/subject/components/LeftStatistical/components/riverNetwork/components/RiverQuality/index.vue'
import PieRiverway from '@/views/screen/components/PieRiverway/index.vue'
import LineRainTrend from '@/views/statistical_analysis/rainRegimen/components/LineRainTrend/index.vue'
import ScreenPanelTitle from '@/views/screen/commonModules/screenPanelTitle/index.vue'
import SluicePumpPreview from '@/components/common/SluicePumpPreview/index.vue'
import BarRainAnalysis from '@/views/statistical_analysis/rainRegimen/components/BarRainAnalysis/index.vue'
import BarSimpleChart from '@/components/chart/BarSimpleChart/index.vue'
import WeatherImg from '@/assets/imgs/weather.jpg'
import PumpImg from '@/assets/imgs/pump.jpg'
export default {
  name: 'deskPage',
  components: {
    'river-quality': RiverQuality,
    'line-urban-management': LineUrbanManagement,
    'pie-riverway': PieRiverway,
    'bar-simple-chart': BarSimpleChart,
    'bar-rain-analysis': BarRainAnalysis,
    'line-rain-trend': LineRainTrend,
    'screen-panel-title': ScreenPanelTitle,
    'sluice-pump-preview': SluicePumpPreview,
    'head-nav': HeadNav
  },
  props: {},
  data() {
    return {
      chart_data: {
        theme: ['#fd836c', '#ff9c65'],
        yUnit: 'mm',
        xUnit: 'h',
        xAxisData: ['0', '2', '4', '6', '8', '10', '12', '14', '16', '18', '20', '22'],
        data: [3, 1, 2, 3, 2, 2, 1, 3, 3, 2, 3, 2, 3, 1, 2, 3, 2, 2, 1, 3, 3, 2, 3, 2]
      },
      weatherImgUrl: WeatherImg,
      pumpImgUrl: PumpImg,
      screenfull_state: false,
      moveBox: null,
      boxWrapper: null,
      moveItem: null,
      resizePosition: null,
      zIndex: 0

    }
  },
  created() { },
  mounted() {
    this.$refs._bar_simple_chart.initChart({
      theme: ['#fd836c', '#ff9c65'],
      yUnit: 'mm',
      xUnit: 'h',
      xAxisData: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23'],
      data: [3, 1, 2, 3, 2, 2, 1, 3, 3, 2, 3, 2, 3, 1, 2, 3, 2, 2, 1, 3, 3, 2, 3, 2]
    })
    this.initPage()
  },
  methods: {
    initPage() {
      this.moveBox = document.getElementById('moveBox')
      this.boxWrapper = document.getElementById('moveBoxWrapper')
      this.moveItem = this.boxWrapper.getElementsByTagName('li')
      this.resizePosition = []
      this.zIndex = 0
      // 布局转换
      for (var i = 0; i < this.moveItem.length; i++) {
        this.moveItem[i].index = i
        this.moveItem[i].style.top = this.moveItem[i].offsetTop + 'px'
        this.moveItem[i].style.left = this.moveItem[i].offsetLeft + 'px'
        this.resizePosition.push({ 'left': this.moveItem[i].offsetLeft, 'top': this.moveItem[i].offsetTop })
      }
      for (let i = 0; i < this.moveItem.length; i += 1) {
        this.moveItem[i].style.position = 'absolute'
        this.drap(this.moveItem[i])
      }
    },
    drap(obj) {
      console.log('​drap -> obj', obj)
      // 拖拽函数
      let _this = this
      let drapDom = obj.getElementsByTagName('h6')[0]
      drapDom.onmousedown = function (e) {
        var oNear = null
        let __this = this
        var ev = e || window.event
        var disX = ev.clientX - obj.offsetLeft - _this.boxWrapper.offsetLeft
        var disY = ev.clientY - obj.offsetTop - _this.boxWrapper.offsetTop
        this.className = 'cur drap_button'
        obj.style.zIndex = _this.zIndex++
        if (obj.setCapture) {
          obj.setCapture()
        } else {
          window.captureEvents(Event.MOUSEMOVE)
        }
        ev.cancelBubble = true// 阻止事件冒泡，防止冲突

        document.onmousemove = function (e) {
          var ev = e || window.event
          var w = ev.clientX - disX - _this.boxWrapper.offsetLeft
          var h = ev.clientY - disY - _this.boxWrapper.offsetTop;
          (w < 0) && (w = 0);
          (w >= (_this.boxWrapper.offsetWidth - obj.offsetWidth)) && (w = (_this.boxWrapper.offsetWidth - obj.offsetWidth));
          (h < 0) && (h = 0);
          (h >= (_this.boxWrapper.offsetHeight - obj.offsetHeight)) && (h = (_this.boxWrapper.offsetHeight - obj.offsetHeight))

          obj.style.top = Number(h) - 23 + 'px'
          obj.style.left = Number(w) - 25 + 'px'
          for (var i = 0; i < _this.moveItem.length; i++) {
            _this.moveItem[i].className = 'item-wrapper move-box__item'
          }
          oNear = _this.findObj(obj)
          oNear && (oNear.className = 'hig item-wrapper move-box__item')
        }
        document.onmouseup = function () {
          if (obj.releaseCapture) {
            obj.releaseCapture()
          } else {
            window.releaseEvents(Event.MOUSEMOVE | Event.MOUSEUP)
          }
          __this.className = 'drap_button'
          document.onmouseup = null
          document.onmousemove = null
          for (var i = 0; i < _this.moveItem.length; i++) {
            _this.moveItem[i].className = 'item-wrapper move-box__item'
          }
          if (oNear) {
            oNear.style.zIndex = _this.zIndex++
            _this.startMove(obj, _this.resizePosition[oNear.index])
            _this.startMove(oNear, _this.resizePosition[obj.index])
            var temp = oNear.index
            oNear.index = obj.index
            obj.index = temp
          } else {
            _this.startMove(obj, _this.resizePosition[obj.index])
          }

          return false
        }
      }
    },
    // 找出相遇点中最近的元素
    findObj(obj) {
      var arr1 = []
      var arr2 = []
      var res = null
      var minnum = 9999999
      var minLi = null

      for (var i = 0; i < this.moveItem.length; i++) {
        res = this.isButt(obj, this.moveItem[i])
        if (this.moveItem[i] !== obj && res) {
          arr1.push(this.dian(obj, this.moveItem[i]))
          arr2.push(this.moveItem[i])
        }
      }

      for (let i = 0; i < arr1.length; i++) {
        if (arr1[i] < minnum) {
          minnum = arr1[i]
          minLi = arr2[i]
        }
      }
      return minLi
    },
    // 求亮点之间的距离
    dian(obj1, obj2) {
      var a = (obj1.offsetLeft + obj1.offsetWidth / 2) - (obj2.offsetLeft + obj2.offsetWidth / 2)
      var b = (obj1.offsetTop + obj1.offsetHeight / 2) - (obj2.offsetTop + obj2.offsetHeight / 2)
      return Math.sqrt(a * a + b * b)
    },
    // 碰撞检测
    isButt(aObj, bObj) {
      var a1 = aObj.offsetLeft
      var b1 = aObj.offsetTop
      var c1 = aObj.offsetLeft + aObj.offsetWidth
      var d1 = aObj.offsetTop + aObj.offsetHeight

      var a2 = bObj.offsetLeft
      var b2 = bObj.offsetTop
      var c2 = bObj.offsetLeft + bObj.offsetWidth
      var d2 = bObj.offsetTop + bObj.offsetHeight
      if (a2 > c1 || b2 > d1 || a1 > c2 || b1 > d2) {
        return false
      } else {
        return true
      }
    },
    // 获取最终样式
    getStyle(obj, attr) {
      return parseFloat(obj.currentStyle ? obj.currentStyle[attr] : getComputedStyle(obj, null)[attr])
    },

    // 运动框架
    startMove(obj, pos, onEnd) {
      clearInterval(obj.timer)
      let _this = this
      obj.timer = setInterval(function () {
        _this.doMove(obj, pos, onEnd)
      }, 30)
    },
    doMove(obj, pos, onEnd) {
      var iCurL = this.getStyle(obj, 'left')
      var iCurT = this.getStyle(obj, 'top')
      var iSpeedL = (pos.left - iCurL) / 5
      var iSpeedT = (pos.top - iCurT) / 5
      iSpeedL = iSpeedL > 0 ? Math.ceil(iSpeedL) : Math.floor(iSpeedL)
      iSpeedT = iSpeedT > 0 ? Math.ceil(iSpeedT) : Math.floor(iSpeedT)
      if (pos.left === iCurL && pos.top === iCurT) {
        clearInterval(obj.timer)
        onEnd && onEnd()
      } else {
        obj.style.left = iCurL + iSpeedL + 'px'
        obj.style.top = iCurT + iSpeedT + 'px'
      }
    }

  },
  computed: {},
  watch: {}
}
</script>

<style lang="scss" scoped>
@import "./../../assets/style/desk.scss";
.desk-page {
  .page-main {
    .move-box {
      width: 1710px;
      overflow: hidden;
      zoom: 1;
      margin: 0 auto;
      &__wrapper {
        width: 1710px;
        overflow: hidden;
        zoom: 1;
        position: relative;
        margin: -23px -25px 0;
        height: 860px;
      }
      &__item {
        width: 520px;
        margin: 23px 25px;
        padding: 0 20px 20px;
        height: 380px;
        float: left;
        font-weight: bolder;
        color: #fff;
        background: rgba(0, 255, 255, 0.06);
        text-align: center;
        user-select: none;
        .item-content {
          background: rgba(0, 255, 255, 0.06);
          height: calc(100% - 46px);
          display: flex;
          align-items: center;
          justify-content: center;
        }
        .tool-tip {
          display: none;
          position: absolute;
          left: 50%;
          top: 0;
          border-radius: 4px;
          padding: 5px 10px;
          z-index: 2000;
          font-size: 12px;
          line-height: 1.2;
          min-width: 10px;
          word-wrap: break-word;
          transform: translate(-50%, -20px);
          font-size: 12px;
          color: #197ae7;
          background: rgba(0, 0, 0, 0.5);
        }
        .tool-tip:after {
          content: "";
          display: block;
          position: absolute;
          box-sizing: content-box;
          width: 0;
          height: 0;
          border: 10px solid transparent;
          border-top-color: rgba(0, 0, 0, 0.5);
          transform: translate(-50%, 10px);
          margin-bottom: -10px;
          bottom: 0;
          left: 50%;
        }
        .tool-tip.show {
          display: block;
        }
        .drap_button {
          visibility: hidden;
          position: absolute;
          z-index: 100;
          cursor: move;
          left: 50%;
          top: 0;
          transform: translate(-50%, -5px) scale(0);
          .iconfont {
            color: #197ae7;
            font-weight: lighter;
            font-size: 50px;
          }
        }
        // .drap_button:hover + .tool-tip {
        //   display: block;
        // }
        img {
          width: 100%;
          height: 100%;
        }

        &:hover {
          .drap_button {
            visibility: visible;
            transform: translate(-50%, -5px) scale(1);
          }
          .tool-tip {
            display: block;
          }
        }
      }
      .mounse-down.drap_button {
        .iconfont {
          color: #197ae7;
          font-size: 52px;
        }
      }
      .cur {
        cursor: move;
      }
      .hig {
        box-sizing: border-box;
        border: 2px dotted yellow;
      }
    }
  }
}
</style>
