# Amap 轨迹回放和vue

## 一.完成版 (vue3 + amap2.0) 大功告成

放了一份代码在文件中

### x.待处理问题
1. 小车图片显示
2. 行驶过的地方改变颜色
3. 在uniapp, h5中使用

### 1.在buildadmin系统中, 安装对应扩展 `pnpm`

- 由于不记得执行过程了,好像是 `pnpm install`
- 但是由于 `style.css`之类的问题提示不存在, 又单独安装了一下

```bash
pnpm install @vuemap/vue-amap view-ui-plus
```

### 2.package.json

```js
{
    "name": "build-admin",
    "version": "2.0.4",
    "license": "Apache-2.0",
    "scripts": {
        "dev": "vite --force",
        "build": "vite build && esno ./src/utils/build.ts",
        "lint": "eslint --ext .js,.jsx,.ts,.vue src",
        "lint-fix": "eslint --ext .js,.jsx,.ts,.vue src --fix",
        "format": "npx prettier --write ."
    },
    "dependencies": {
        "@amap/amap-jsapi-loader": "^1.0.1",
        "@element-plus/icons-vue": "2.1.0",
        "@vuemap/vue-amap": "^2.0.24",
        "@vueuse/core": "10.2.0",
        "axios": "1.4.0",
        "countup.js": "2.6.2",
        "echarts": "5.4.2",
        "element-plus": "2.3.7",
        "esno": "0.16.3",
        "font-awesome": "4.7.0",
        "lodash-es": "4.17.21",
        "mitt": "3.0.0",
        "nprogress": "0.2.0",
        "pinia": "2.1.4",
        "pinia-plugin-persistedstate": "3.1.0",
        "qrcodejs2-fix": "^0.0.1",
        "screenfull": "6.0.2",
        "sortablejs": "1.15.0",
        "v-code-diff": "1.5.1",
        "vue": "3.3.4",
        "vue-i18n": "9.2.2",
        "vue-qr": "4.0.9",
        "vue-router": "4.2.2",

        "vue-amap": "^0.5.8",
        "core-js": "^3.8.3",
        "view-ui-plus": "^1.3.14",
    },
    "devDependencies": {
        "@types/lodash-es": "4.17.7",
        "@types/node": "20.3.1",
        "@types/nprogress": "0.2.0",
        "@types/sortablejs": "1.15.1",
        "@typescript-eslint/eslint-plugin": "5.60.0",
        "@typescript-eslint/parser": "5.60.0",
        "@vitejs/plugin-vue": "4.2.3",
        "async-validator": "4.2.5",
        "eslint": "8.43.0",
        "eslint-config-prettier": "8.8.0",
        "eslint-plugin-vue": "9.15.1",
        "less": "^4.2.0",
        "prettier": "2.8.8",
        "sass": "1.63.6",
        "typescript": "5.1.3",
        "vite": "4.3.9",
        "vue-eslint-parser": "9.3.1"
    }
}
```

### 3.main.js中初始化

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import { loadLang } from '/@/lang/index'
import { registerIcons } from '/@/utils/common'
import ElementPlus from 'element-plus'
import mitt from 'mitt'
import pinia from '/@/stores/index'
import { directives } from '/@/utils/directives'
import 'element-plus/dist/index.css'
import 'element-plus/theme-chalk/display.css'
import 'font-awesome/css/font-awesome.min.css'
import '/@/styles/index.scss'
import VueQrcode from 'vue-qrcode-directive'
// modules import mark, Please do not remove.

import '@vuemap/vue-amap/dist/style.css'
import 'view-ui-plus/dist/styles/viewuiplus.css'
import ViewUIPlus from 'view-ui-plus'
import VueAMap, { initAMapApiLoader } from '@vuemap/vue-amap';

async function start() {
    const app = createApp(App)
    app.use(pinia)

    // 全局语言包加载
    await loadLang(app)

    app.use(router)
    app.use(ElementPlus)

    app.directive('qrcode', VueQrcode)

    // 全局注册 
    directives(app) // 指令
    registerIcons(app) // icons

    initAMapApiLoader({
        // 高德的key
        key: 'you_key',
        // 插件集合
        plugin: [],
        AMapUI: {},
        // 高德 sdk 版本，默认为 1.4.4
        v: '2.0'
    })

    app.use(VueAMap)
    app.use(ViewUIPlus)

    app.mount('#app')

    app.config.globalProperties.eventBus = mitt()
}

start()
```

### 3.在页面中使用

```ts
<template>
    <div class="map-outbox">
      <!-- 地图 -->
      <div id="container"></div>
      <!-- 进度条 -->
      <div class="map-control">
        <Icon v-if="!isPlay" class="play-icon" type="ios-play" @click="isPlay = true; navgControl(playIcon)" />
        <Icon v-else class="play-icon" type="ios-pause" @click="isPlay = false; navgControl('pause')" />
        <span class="passed-time">{{ passedTime }}</span>
        <Slider class="map-slider" v-model="sliderVal" @on-change="sliderChange" :tip-format="hideFormat" :step="0.0001">
        </Slider>
        <div class="map-times" @mouseenter="isTimesChoose = true" @mouseleave="isTimesChoose = false">
          <div class="times-show">倍速 {{ times }}</div>
          <div class="choose-box">
            <ul v-show="isTimesChoose">
              <li v-for="item in speedList" :key="item" :class="{ active: times === item }" @click="changeSpeed(item)">倍速
                {{ item }}</li>
            </ul>
          </div>
        </div>
        <span class="passed-time">{{ totalTime }}</span>
      </div>
    </div>
  </template>
  
  <script setup>
  import { linePath, marks } from '/@/api/public/index'

  import car from "/@/public/car.png";
  // import VueAMap, { initAMapApiLoader } from '@vuemap/vue-amap';
  import { lazyAMapApiLoaderInstance } from '@vuemap/vue-amap';
  import { onMounted, onUnmounted, ref, watch } from 'vue'
  let map = ref(null)
  let isOnSlider = ref(false) //是否为手动鼠标拉动进度条
  let playIcon = ref('resume');  //开始按钮是重新开始还是继续
  let isTimesChoose = ref(false) //选择速度弹窗的open/close
  let passedTime = ref('00:00:00') //已经走了的时间
  let totalTime = ref('00:00:00') //已经走了的时间
  let isPlay = ref(false)
  let sliderVal = ref(0) // 进度条滑动速度
  let speed = ref(100) //初始速度，km、h
  let times = ref(1) //几倍速度播放
  let navgtr = ref(null)  //巡航器
  let speedList = ref([8, 4, 2, 1])
  let pathList = ref([])
  let trackList = ref([])
  let pathSimplifierIns = ref(null)
  
  let emits = defineEmits(['handleqktraV2IdId'])
  onMounted(() => {
    let param = {
      resizeEnable: true,
      zoom: 15,
      center: [116.47984, 39.998524],
    };
    lazyAMapApiLoaderInstance.then(() => {
      // your code ...
      map.value = new AMap.Map("container", param);
      init();
    });
  })
  
  const init = () => {
    // 创建起始和经过的icon
    let iconList = [];
    marks.forEach(item => {
      addMarker(item);
    });
  
    // 轨迹
    let list = linePath;
    let len = list.length;
    let startPoint = list[0];
    let endPoint = list[len - 1];
    pathList.value.splice(0, pathList.value.length);
    list.forEach(item => {
      pathList.value.push([item.longitude, item.latitude]);
      item.stampTime = new Date(item.time).getTime(); //当前时间戳
    });
    // 下一段路程经历了多少s,intervalTime,nextDistance:下一段路程：m,nextDistance:下一段路速度：km/h
    list.forEach((item, i) => {
      let next = list[i + 1];
      if (next) {
        item.intervalTime = (next.stampTime - item.stampTime) / 1000;
        item.nextDistance = distanceFun(
          [item.longitude, item.latitude],
          [next.longitude, next.latitude]
        );
        item.nextSpeed =
          item.nextDistance / 1000 / (item.intervalTime / 60 / 60);
      }
    });
    // 订单记录总时间，hh:mm:ss
    totalTime.value = getTime(
      (endPoint.stampTime - startPoint.stampTime) / 1000
    );
    trackList.value = list;
    setPath();
    drag();
  }
  
  const setPath = () => {
    AMapUI.load(["ui/misc/PathSimplifier", "lib/$"], function (
      PathSimplifier
    ) {
      if (!PathSimplifier.supportCanvas) {
        console.log("当前环境不支持 Canvas！");
        return;
      }
      let len = trackList.value.length;
      let startPoint = trackList.value[0];
      let endPoint = trackList.value[len - 1];
      // 轨迹总数
      function onload() {
        pathSimplifierIns.value.renderLater();
      }
      function onerror() {
        infoContent;
        console.log("图片加载失败！");
      }
  
      // 历史轨迹巡航器
      pathSimplifierIns.value = new PathSimplifier({
        zIndex: 100,
        //autoSetFitView:false,
        map: map.value, //所属的地图实例
  
        getPath: function (pathData) {
          return pathData.path;
        },
        getHoverTitle: function (pathData) {
          return pathData.index;
        },
        autoSetFitView: true,
        // 巡航器样式
        renderOptions: {
          pathNavigatorStyle: {
            initRotateDegree: 0,
            width: 20,
            height: 32,
            autoRotate: true,
            lineJoin: "round",
            content: PathSimplifier.Render.Canvas.getImageContent(
              car,
              onload,
              onerror
            ),
            fillStyle: null,
            strokeStyle: null,
            lineWidth: 1,
            pathLinePassedStyle: {
              lineWidth: 6,
              strokeStyle: "#2cdf4d"
            }
          },
          // 这个会修改所有颜色
          pathLineStyle: {
            lineWidth: 6,
            strokeStyle: "#2e9c08"
          },
          pathLineHoverStyle: {
            lineWidth: 0,
            borderWidth: 0
          },
          pathLineSelectedStyle: {
            lineWidth: 6,
            borderWidth: 0,
            strokeStyle: "#2e9c08"
          },
          pathTolerance: 0,
          keyPointTolerance: -1,
          renderAllPointsIfNumberBelow: 0 //绘制路线节点，如不需要可设置为-1
        }
      });
  
      //历史轨迹巡航器设置数据
      pathSimplifierIns.value.setData([
        {
          name: "轨迹",
          path: pathList.value
        }
      ]);
      pathSimplifierIns.value.setFitView(-1);
  
      let startSpeed = speedFun(
        pathList.value[0],
        pathList.value[1],
        startPoint.intervalTime
      );
      startSpeed === 0 && (startSpeed = 0.1);
  
      //对第一条线路（即索引 0）创建一个巡航器
      navgtr.value = pathSimplifierIns.value.createPathNavigator(0, {
        loop: false, //循环播放
        speed: startSpeed * times.value //巡航速度，单位千米/小时
      });
      //构建自定义信息窗体
      let infoContent = `<p class="info-window">时间：<span>${startPoint.time} `;
      let infoWindow = new AMap.InfoWindow({
        anchor: "bottom-center",
        content: infoContent
      });
  
      infoWindow.open(map.value, pathList.value[0]);
      // 移动过程中
      navgtr.value.on("move", function () {
        playIcon.value = "resume";
        let idx = this.getCursor().idx; //走到了第几个点
        let tail = this.getCursor().tail; //至下一个节点的比例位置
        let totalIdx = idx + tail;
  
        // 计算下一个距离速度
        let point = trackList.value[idx];
        if (idx < len - 1) {
          point.nextSpeed === 0 && (point.nextSpeed = 0.1);
          navgtr.value.setSpeed(point.nextSpeed * times.value);
        }
  
        // 剩余公里数，窗体随时移动展示
        point &&
          point.time &&
          infoWindow.setContent(
            `<p class="info-window">时间：<span>${point.time}`
          );
        infoWindow.open(map.value, navgtr.value.getPosition());
        // 进度条实时展示tail
        !isOnSlider.value && (sliderVal.value = (totalIdx / len) * 100);
        // 已经播放时间
        // let sTime = (pointObj.stampTime-startStampTime)/1000;
        let sTime = parseInt(
          (((endPoint.stampTime - startPoint.stampTime) / 1000) *
            sliderVal.value) /
          100
        );
  
        passedTime.value = getTime(sTime);
        // 如果到头了，回到初始状态
        if (navgtr.value.isCursorAtPathEnd()) {
          playIcon.value = "start";
          isPlay.value = false;
          sliderVal.value = 100;
          passedTime.value = totalTime.value;
        }
      });
  
      navgtr.value.on("start resume", function () {
  
        navgtr.value._startTime = Date.now();
        navgtr.value._startDist = this.getMovedDistance();
      });
      navgtr.value.on("stop pause", function () {
        navgtr.value._movedTime = Date.now() - navgtr.value._startTime;
        navgtr.value._movedDist =
          this.getMovedDistance() - navgtr.value._startDist;
      });
    });
  }
  
  // 开始、暂停、继续等操作
  const navgControl = (action) => {
    if (action === "start") {
      sliderVal.value = 0;
      passedTime.value = "00:00:00";
      setTimeout(() => {
        navgtr.value[action]();
      }, 300);
    } else {
      navgtr.value[action]();
    }
  }
  
  //     // 格式化时间
  const getTime = (sTime) => {
    let ss;
    let mm = "00";
    let hh = "00";
    if (sTime > 60) {
      let s = sTime % 60;
      ss = s > 9 ? s : "0" + s;
      let mTime = parseInt(sTime / 60);
      if (mTime > 60) {
        let m = mTime % 60;
        mm = m > 9 ? m : "0" + m;
        hh = parseInt(mTime / 60);
      } else {
        mm = mTime > 9 ? mTime : "0" + mTime;
      }
    } else {
      ss = sTime > 9 ? sTime : "0" + sTime;
    }
    return hh + ":" + mm + ":" + ss;
  }
  const hideFormat = () => {
    return null;
  }
  // 计算两个坐标点之间的距离
  const distanceFun = (point1, point2) => {
    // point1:[longitude,latitude]
    let p1 = new AMap.LngLat(point1[0], point1[1]);
    let p2 = new AMap.LngLat(point2[0], point2[1]);
    let distance = Math.round(p1.distance(p2));
    return distance;
  }
  const speedFun = (point1, point2, time) => {
    // point1,point2:经纬度数组，time:时间，s
    let distance = distanceFun(point1, point2);
    if (distance === 0) {
      return 0;
    } else {
      let speed = distance / 1000 / (time / 60 / 60);
      // speed:km/h
      return speed;
    }
  }
  
  const drag = (isRemove) => {
    let el = document.getElementsByClassName("ivu-slider-button-wrap")[0];
    let el2 = document.getElementsByClassName("ivu-slider-wrap")[0];
  
    if (isRemove) {
      el && el.removeEventListener("mousedown", openSlider.value, false);
      document.removeEventListener("mouseup", closeSlider.value, false);
      el2 && el2.removeEventListener("click", sliderChange.value, false);
      return false;
    }
    el2.addEventListener("click", sliderChange.value, false);
    el.addEventListener("mousedown", openSlider.value, false);
    // 此处用document是因为，滑动较为随意时，mouseup可能不是作用在el上
    document.addEventListener("mouseup", closeSlider.value, false);
  }
  
  
  const openSlider = () => {
    isOnSlider.value = true;
  }
  
  const closeSlider = () => {
    isOnSlider.value = false;
  }
  
  // 修改倍速
  const changeSpeed = (item) => {
    isTimesChoose.value = false;
    times.value = item;
  }
  
  const sliderChange = (val) => {
    let newVal = typeof val === "number" ? val : sliderVal.value;
    let num = parseInt((newVal / 100) * pathList.value.length);
    let decimal =
      String((newVal / 100) * pathList.value.length).split(".")[1] || 0;
    navgtr.value.moveToPoint(num, Number("0." + decimal));
    pathSimplifierIns.value.renderLater();
  }
  
  const addMarker = (item) => {
    let marker = new AMap.Marker({
      icon:
        "//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
      position: [item.longitude, item.latitude],
      offset: new AMap.Pixel(-13, -30)
    });
    marker.setMap(map.value);
  }
  
  // 新增假如当前是子组件关闭的时候判断是播放还是其他状态，防止setSpeed报错null
  const cancelsubmitfotm = () => {
    if (isPlay.value) {
      isPlay.value = false;
      navgControl("pause");
    }
    navgtr.value = null;
    drag(true);
    emits("handleqktraV2IdId", this.traV2Id)
    dialogFormVisible.value = false;
  }
  onUnmounted(() => {
    navgtr.value = ref(null);
    drag(true);
  })
  
  watch(() => sliderVal.value,
    (val) => {
      if (!isOnSlider.value) {
        return false;
      }
      sliderChange(val);
    },
    {
      deep: true,
    })
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style>
  .map-outbox {
    width: 1240px;
    padding: 20px;
    border: 1px solid #ddd;
    margin: auto;
  }
  
  #container {
    width: 1200px;
    height: 600px;
  }
  
  .amap-info-close {
    display: none;
  }
  
  .info-window span {
    color: #2e75f1;
  }
  
  .ivu-slider-bar {
    color: #fff;
    background: #fff;
    transition: all 0.2s linear;
  }
  
  .ivu-slider-button-wrap {
    transition: all 0.2s linear;
    top: -5px;
  }
  
  .ivu-slider-wrap {
    background-color: rgba(255, 255, 255, 0.5);
  }
  
  .ivu-slider-button {
    border: none;
    box-shadow: 0 0 0 3.5px rgba(230, 230, 230, 0.4);
  }
  
  .order-map,
  .map-outbox {
    position: relative;
    min-height: 70vh;
  }
  
  .color-tag {
    position: absolute;
    top: 15px;
    left: 15px;
    z-index: 200;
    font-size: 14px;
  }
  
  .color-tag li {
    display: inline-block;
    border-radius: 4px;
    width: 100px;
    height: 36px;
    line-height: 36px;
    text-align: center;
    background-color: #fff;
    cursor: pointer;
    margin-right: 2px;
  }
  
  .color-tag li.color-org {
    background-color: #ff8533;
    color: #fff;
  }
  
  .color-tag li.color-green {
    background-color: #46c51a;
    color: #fff;
  }
  
  .map-control {
    position: absolute;
    bottom: 0;
    width: 100%;
    left: 0;
    z-index: 200;
    height: 80px;
    line-height: 80px;
    color: #fff;
    background-image: linear-gradient(to top, rgba(0, 0, 0, 0.7), transparent);
    padding: 0 40px;
  }
  
  .play-icon {
    font-size: 36px;
  }
  
  .map-slider {
    display: inline-block;
    width: 75%;
    margin-left: 15px;
    position: relative;
    top: 14px;
  }
  
  .passed-time {
    position: relative;
    top: 2px;
    display: inline-block;
    margin-left: 15px;
    font-size: 14px;
  }
  
  .map-times {
    display: inline-block;
    position: relative;
    margin-left: 15px;
  }
  
  .map-times .times-show {
    padding: 0 10px;
    line-height: 24px;
    font-size: 13px;
    border: 1px solid #fff;
    border-radius: 4px;
    cursor: default;
  }
  
  .map-times .choose-box {
    position: absolute;
    top: -135px;
    left: -6px;
    height: 162px;
    transition: all 0.5s linear;
  }
  
  .map-times ul {
    background: rgba(0, 0, 0, 0.7);
    padding: 10px;
    width: 70px;
    text-align: center;
    border-radius: 3px;
  }
  
  .map-times li {
    height: 26px;
    line-height: 26px;
    cursor: pointer;
  }
  
  .map-times li.active {
    color: #ff8533;
  }
  
  .map-times li:hover {
    font-size: 13px;
  }
  
  /* .marker {
    position: absolute;
    top: -20px;
    right: -118px;
    color: #fff;
    padding: 4px 10px;
    box-shadow: 1px 1px 1px rgba(10, 10, 10, 0.2);
    white-space: nowrap;
    font-size: 12px;
    font-family: "";
    background-color: #25a5f7;
    border-radius: 3px;
  } */
  .amap-icon img {
    width: 25px;
    height: 34px;
  }
  </style>
```

## 方案1: 直接使用AmapUI或者是顾洋洋项目来实现

使用 AMapLoader 加载 AMapUI就可以了，参考以下官方demo  

```
https://lbs.amap.com/api/javascript-api-v2/guide/abc/amap-vue  
https://lbs.amap.com/demo/amap-ui/demos/amapui-amaploader/amapui-amaploader1
```


另外, 这几有个基于amap2.0封装的vue项目, 然后第二个链接展示了如何在这个项目使用AmapUI

```
https://vue-amap.guyixi.cn/zh-cn/base/amap.html - 顾洋洋项目
https://blog.csdn.net/lovexiuwei/article/details/125426412
```

## 方案2: 将vue2版本的巡航轨迹, 改写成vue3版本

```
https://github.com/limuen/trackdisplay
https://github.com/dodobird2077/TrackDisplay
```

应公司业务要求，给轨迹回放加个滚动条，一开始我采用的是高德地图的折线绘制，但是如果要加上进度条并不太不合适，拖动滚动条，点位没有恢复的API，后来决定使用巡航轨迹，巡航轨迹支持更丰富的轨迹API，使用起来也更方便。

所用技术：vue，高德地图（amap），iview-slider

绘制轨迹使用的是高德地图的 轨迹展示 功能

对应demo地址：https://lbs.amap.com/api/amap-ui/demos/amap-ui-pathsimplifier/simple-demo

进度条使用的是iview中的滑块，如果想换element或者其他的组件库的slider自行更换Slider标签即可

使用方法 (`第三步, 构建成html,js静态资源, 可能就是方案3的实现方式, 或者独立部署一个前端项目`)

```bash
npm install
npm run serve
npm run build
```

- 主要代码

### 1. main.js

```js
import Vue from 'vue'
import App from './App.vue'
import iView from 'iview'
import AMap from 'vue-amap'
import 'iview/dist/styles/iview.css'

Vue.config.productionTip = false
Vue.use(iView)
// Vue.use(AMap)

AMap.initAMapApiLoader({
  // 高德的key
  key: '-',
  // 插件集合
  plugin: ['AMapManager', 'AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor',
   'AMap.CircleEditor','Geocoder','Geolocation', 'AMap.MarkerClusterer','AMap.PolyEditor', 'AMap.CircleEditor', 'AMap.MouseTool', 'AMap.Driving',
    'AMap.CitySearch', 'AMap.InfoWindow', 'AMap.LngLat', 'AMap.DistrictSearch', 'AMap.TileLayer.Traffic', 'AMap.Heatmap', 'AMap.Autocomplete',
     'AMap.PlaceSearch'],
  // 高德 sdk 版本，默认为 1.4.4
  v: '2'
})

new Vue({
  render: h => h(App),
}).$mount('#app')
```

### 2. package.json

```js
{
  "name": "gdpath",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "vue": "^2.6.11",
    "iview": "^3.3.2",
    "vue-amap": "^0.5.8"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-eslint": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "babel-eslint": "^10.1.0",
    "eslint": "^6.7.2",
    "eslint-plugin-vue": "^6.2.2",
    "vue-template-compiler": "^2.6.11"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}
```

### 3. 功能页面

```html
<template>
  <div class="map-outbox">
    <!-- 地图 -->
    <div id="container"></div>
    <!-- 进度条 -->
    <div class="map-control">
      <Icon
        v-if="!isPlay"
        class="play-icon"
        type="ios-play"
        @click="isPlay=true;navgControl(playIcon)"
      />
      <Icon v-else class="play-icon" type="ios-pause" @click="isPlay=false;navgControl('pause')" />
      <span class="passed-time">{{passedTime}}</span>
      <Slider class="map-slider" v-model="sliderVal" :tip-format="hideFormat" :step="0.0001"></Slider>
      <div class="map-times" @mouseenter="isTimesChoose=true" @mouseleave="isTimesChoose=false">
        <div class="times-show">倍速 {{times}}</div>
        <div class="choose-box">
          <ul v-show="isTimesChoose">
            <li
              v-for="item in speedList"
              :key="item"
              :class="{active:times===item}"
              @click="changeSpeed(item)"
            >倍速 {{item}}</li>
          </ul>
        </div>
      </div>
      <span class="passed-time">{{totalTime}}</span>
    </div>
  </div>
</template>

<script>
import { linePath, marks } from "./index.js";
import car from "../assets/car.png";

export default {
  name: "HelloWorld",
  data() {
    return {
      map: null,
      isOnSlider: false, //是否为手动鼠标拉动进度条
      playIcon: "resume", //开始按钮是重新开始还是继续
      isTimesChoose: false, //选择速度弹窗的open/close
      passedTime: "00:00:00", //已经走了的时间
      totalTime: "00:00:00", //已经走了的时间
      isPlay: false, //是否为播放
      sliderVal: 0, //进度条滑动速度
      speed: 100, //初始速度，km、h
      times: 1, //几倍速度播放
      navgtr: null, //巡航器
      speedList: [8, 4, 2, 1],
      pathList: [],
      trackList: []
    };
  },
  mounted() {
    let param = {
      resizeEnable: true,
      zoom: 15
    };
    this.map = new AMap.Map("container", param);
    this.init();
  },
  methods: {
    init() {
      // 创建起始和经过的icon
      let iconList = [];
      marks.forEach(item => {
        this.addMarker(item);
      });

      // 轨迹
      let trackList = linePath;
      let len = trackList.length;
      let startPoint = trackList[0];
      let endPoint = trackList[len - 1];
      this.pathList.splice(0, this.pathList.length);
      trackList.forEach(item => {
        this.pathList.push([item.longitude, item.latitude]);
        item.stampTime = new Date(item.time).getTime(); //当前时间戳
      });
      
      // 下一段路程经历了多少s,intervalTime,nextDistance:下一段路程：m,nextDistance:下一段路速度：km/h
      trackList.forEach((item, i) => {
        let next = trackList[i + 1];
        if (next) {
          item.intervalTime = (next.stampTime - item.stampTime) / 1000;
          item.nextDistance = this.distanceFun(
            [item.longitude, item.latitude],
            [next.longitude, next.latitude]
          );
          item.nextSpeed =
            item.nextDistance / 1000 / (item.intervalTime / 60 / 60);
        }
      });
      // 订单记录总时间，hh:mm:ss
      this.totalTime = this.getTime(
        (endPoint.stampTime - startPoint.stampTime) / 1000
      );
      this.trackList = trackList;
    },
 

  },

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.map-outbox {
  width: 1240px;
  padding: 20px;
  border: 1px solid #ddd;
  margin: auto;
}
#container {
  width: 1200px;
  height: 600px;
}
.amap-info-close {
  display: none;
}
.info-window span {
  color: #2e75f1;
}
.ivu-slider-bar {
  color: #fff;
  background: #fff;
  transition: all 0.2s linear;
}
.ivu-slider-button-wrap {
  transition: all 0.2s linear;
  top: -5px;
}
.ivu-slider-wrap {
  background-color: rgba(255, 255, 255, 0.5);
}
.ivu-slider-button {
  border: none;
  box-shadow: 0 0 0 3.5px rgba(230, 230, 230, 0.4);
}
.order-map,
.map-outbox {
  position: relative;
  min-height: 70vh;
}
.color-tag {
  position: absolute;
  top: 15px;
  left: 15px;
  z-index: 200;
  font-size: 14px;
}
.color-tag li {
  display: inline-block;
  border-radius: 4px;
  width: 100px;
  height: 36px;
  line-height: 36px;
  text-align: center;
  background-color: #fff;
  cursor: pointer;
  margin-right: 2px;
}
.color-tag li.color-org {
  background-color: #ff8533;
  color: #fff;
}
.color-tag li.color-green {
  background-color: #46c51a;
  color: #fff;
}
.map-control {
  position: absolute;
  bottom: 0;
  width: 100%;
  left: 0;
  z-index: 200;
  height: 80px;
  line-height: 80px;
  color: #fff;
  background-image: linear-gradient(to top, rgba(0, 0, 0, 0.7), transparent);
  padding: 0 40px;
}
.play-icon {
  font-size: 36px;
}
.map-slider {
  display: inline-block;
  width: 75%;
  margin-left: 15px;
  position: relative;
  top: 14px;
}
.passed-time {
  position: relative;
  top: 2px;
  display: inline-block;
  margin-left: 15px;
  font-size: 14px;
}
.map-times {
  display: inline-block;
  position: relative;
  margin-left: 15px;
}
.map-times .times-show {
  padding: 0 10px;
  line-height: 24px;
  font-size: 13px;
  border: 1px solid #fff;
  border-radius: 4px;
  cursor: default;
}
.map-times .choose-box {
  position: absolute;
  top: -135px;
  left: -6px;
  height: 162px;
  transition: all 0.5s linear;
}
.map-times ul {
  background: rgba(0, 0, 0, 0.7);
  padding: 10px;
  width: 70px;
  text-align: center;
  border-radius: 3px;
}
.map-times li {
  height: 26px;
  line-height: 26px;
  cursor: pointer;
}
.map-times li.active {
  color: #ff8533;
}
.map-times li:hover {
  font-size: 13px;
}
/* .marker {
  position: absolute;
  top: -20px;
  right: -118px;
  color: #fff;
  padding: 4px 10px;
  box-shadow: 1px 1px 1px rgba(10, 10, 10, 0.2);
  white-space: nowrap;
  font-size: 12px;
  font-family: "";
  background-color: #25a5f7;
  border-radius: 3px;
} */
.amap-icon img {
  width: 25px;
  height: 34px;
}
</style>
```

## 方案3: 直接使用vue2版本的巡航轨迹, 做成单页面使用

## 方案4: 将高德地图的巡航轨迹示例改成vue3版本, 然后再慢慢集成播放暂停之类的功能

```
高德地图的巡航轨迹示例
https://lbs.amap.com/demo/amap-ui/demos/amap-ui-pathsimplifier/simple-demo
```
