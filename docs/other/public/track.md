# Amap 轨迹回放和vue


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
