
## 一.AMAP 高德地图

- 说明: 用的是带路由的自定义页面 `web\src\views\backend\goods\amap.vue`
- 可以继续研究下怎么样不需要 el-dialog 也能显示, 新增一个, 这个保留可以嵌入其他页面

```html
<template>
    <el-dialog
        v-model="showAmap"
        class="ba-operate-dialog"
        width="70%"
        height="100%"
    >
        <div id="map-container" style="width: 100%; height: 100%" />
    </el-dialog>
</template>

<script>
import { ref, onMounted, shallowRef } from 'vue'
import AMapLoader from '@amap/amap-jsapi-loader'
import { ElNotification } from 'element-plus'

export default {
    setup() {
        const showAmap = ref(true)
        const map = shallowRef(null)
        const marker = ref(null)

        const initMap = async (stationCenter) => {
            AMapLoader.load({
                key: 'you_key', // 你的高德地图key
                version: '2.0',
                plugins: ['AMap.Marker'],
            })
                .then((AMap) => {
                    map.value = new AMap.Map('map-container', {
                        viewMode: '3D', // 是否为3D地图模式
                        zoom: 15, // 初始化地图级别
                        resizeEnable: true, //是否监控地图容器尺寸变化
                        mapStyle: 'amap://styles/normal',
                        center: stationCenter, // 初始化地图中心点位置
                        showMarker: true, // 定位成功后在定位到的位置显示点标记，默认：true
                    })
                    //添加标记
                    const marker = new AMap.Marker({
                        position: stationCenter,
                        icon: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_bs.png',
                        draggable: true,
                        // map: map.value,
                    })
                    map.value.add(marker)
                })
                .catch((e) => {
                    console.log(e)
                })
        }

        onMounted(() => {
            window._AMapSecurityConfig = {
                securityJsCode: 'you_key',
            }
            initMap([116.361124, 39.959828]) // 初始化地图, 中心点
        })

        return {
            showAmap,
            marker,
            map,
            initMap,
        }
    },
}
</script>

<style>
.amap {
    margin: 10px 0;
}
#map-container {
    height: 400px;
    width: 100%;
}
</style>
```