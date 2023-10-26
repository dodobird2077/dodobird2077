# 首页仪表盘数据修改

- `注意` 那个数据没有响应式更新, 是因为有个数据显示的特效, 调整它的顺序即可 `initCountUp`

## 1.前端代码修改 
`web\src\views\backend\dashboard.vue`

```html
<template>
    
    <!-- 1.登录用户信息 -->
    <div class="default-main">
        <div class="banner">
            <el-row :gutter="10">
                <el-col :md="24" :lg="18">
                    <div class="welcome suspension">
                        <img class="welcome-img" :src="headerSvg" alt="" />
                        <div class="welcome-text">
                            <div class="welcome-title">{{ adminInfo.nickname + t('utils.comma') + getGreet() }}</div>
                            <div class="welcome-note">{{ state.remark }}</div>
                        </div>
                    </div>
                </el-col>
                <el-col :lg="6" class="hidden-md-and-down">
                    <div class="working">
                        <img class="working-coffee" :src="coffeeSvg" alt="" />
                        <div class="working-text">
                            {{ t('dashboard.You have worked today') }}<span class="time">{{ state.workingTimeFormat }}</span>
                        </div>
                        <div @click="onChangeWorkState()" class="working-opt working-rest">
                            {{ state.pauseWork ? t('dashboard.Continue to work') : t('dashboard.have a bit of rest') }}
                        </div>
                    </div>
                </el-col>
            </el-row>
        </div>

        <!-- 2.统计面板 -->
        <div class="small-panel-box">
            <el-row :gutter="20">
                <el-col :sm="12" :lg="6">
                    <div class="small-panel user-reg suspension">
                        <div class="small-panel-title">商品总数</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#8595F4" size="20" name="fa fa-line-chart" />
                                <span id="user_reg_number">{{ indexData.goods_num }}</span>
                            </div>
                            <!-- <div class="content-right">右侧增长率</div> -->
                        </div>
                    </div>
                </el-col>
                <el-col :sm="12" :lg="6">
                    <div class="small-panel file suspension">
                        <div class="small-panel-title">订单总数</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#AD85F4" size="20" name="fa fa-file-text" />
                                <span id="file_number">{{ indexData.order_num }} </span>
                            </div>
                        </div>
                    </div>
                </el-col>
                <el-col :sm="12" :lg="6">
                    <div class="small-panel users suspension">
                        <div class="small-panel-title">库存总数</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#74A8B5" size="20" name="fa fa-users" />
                                <span id="users_number">9486</span>
                            </div>
                        </div>
                    </div>
                </el-col>
                <el-col :sm="12" :lg="6">
                    <div class="small-panel addons suspension">
                        <div class="small-panel-title">待付款数</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#F48595" size="20" name="fa fa-object-group" />
                                <span id="addons_number">875</span>
                            </div>
                        </div>
                    </div>
                </el-col>
                <el-col :sm="12" :lg="6">
                    <div class="small-panel addons suspension">
                        <div class="small-panel-title">待发货数</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#F48595" size="20" name="fa fa-object-group" />
                                <span id="addons_number">875</span>
                            </div>
                        </div>
                    </div>
                </el-col>
                <el-col :sm="12" :lg="6">
                    <div class="small-panel addons suspension">
                        <div class="small-panel-title">已发货数量</div>
                        <div class="small-panel-content">
                            <div class="content-left">
                                <Icon color="#F48595" size="20" name="fa fa-object-group" />
                                <span id="addons_number">875</span>
                            </div>
                        </div>
                    </div>
                </el-col>
            </el-row>
        </div>
       
    </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, reactive, nextTick, onActivated, watch, onBeforeMount, ref } from 'vue'
import headerSvg from '/@/assets/dashboard/header-1.svg'
import coffeeSvg from '/@/assets/dashboard/coffee.svg'
import { CountUp } from 'countup.js'
import { useNavTabs } from '/@/stores/navTabs'
import { index } from '/@/api/backend/dashboard'
import { useI18n } from 'vue-i18n'
import { Local } from '/@/utils/storage'
import { useAdminInfo } from '/@/stores/adminInfo'
import { WORKING_TIME } from '/@/stores/constant/cacheKey'
import { getGreet } from '/@/utils/common'
import { useEventListener } from '@vueuse/core'
import { getIndexData } from '/@/api/public/system'

const indexData = reactive({
  goods_num: 0,
  order_num: 0,
})

getIndexData().then((res)=> {
    indexData.goods_num = res.data.order_num;;
    indexData.order_num = res.data.order_num;
    initCountUp() 
}).catch((error) => {
    console.error("获取数据时出错：", error);
});

let workTimer: number

defineOptions({
    name: 'dashboard',
})

const d = new Date()
const { t } = useI18n()
const navTabs = useNavTabs()
const adminInfo = useAdminInfo()

const state: {
    charts: any[]
    remark: string
    workingTimeFormat: string
    pauseWork: boolean
} = reactive({
    charts: [],
    remark: 'dashboard.Loading',
    workingTimeFormat: '',
    pauseWork: false,
})

index().then((res) => {
    state.remark = res.data.remark
})

const countUpFun = (id: string) => {
    nextTick(() => {
        let value = document.getElementById(id)?.innerText
        if (value) {
            new CountUp(id, parseInt(value), {
                startVal: 0,
                duration: 3,
            }).start()
        }
    })
}

const initCountUp = () => {
    countUpFun('user_reg_number')
    countUpFun('file_number')
    countUpFun('users_number')
    countUpFun('addons_number')
}


const echartsResize = () => {
    nextTick(() => {
        for (const key in state.charts) {
            state.charts[key].resize()
        }
    })
}

const onChangeWorkState = () => {
    const time = parseInt((new Date().getTime() / 1000).toString())
    const workingTime = Local.get(WORKING_TIME)
    if (state.pauseWork) {
        // 继续工作
        workingTime.pauseTime += time - workingTime.startPauseTime
        workingTime.startPauseTime = 0
        Local.set(WORKING_TIME, workingTime)
        state.pauseWork = false
        startWork()
    } else {
        // 暂停工作
        workingTime.startPauseTime = time
        Local.set(WORKING_TIME, workingTime)
        clearInterval(workTimer)
        state.pauseWork = true
    }
}

const startWork = () => {
    const workingTime = Local.get(WORKING_TIME) || { date: '', startTime: 0, pauseTime: 0, startPauseTime: 0 }
    const currentDate = d.getFullYear() + '-' + (d.getMonth() + 1) + '-' + d.getDate()
    const time = parseInt((new Date().getTime() / 1000).toString())

    if (workingTime.date != currentDate) {
        workingTime.date = currentDate
        workingTime.startTime = time
        workingTime.pauseTime = workingTime.startPauseTime = 0
        Local.set(WORKING_TIME, workingTime)
    }

    let startPauseTime = 0
    if (workingTime.startPauseTime <= 0) {
        state.pauseWork = false
        startPauseTime = 0
    } else {
        state.pauseWork = true
        startPauseTime = time - workingTime.startPauseTime // 已暂停时间
    }

    let workingSeconds = time - workingTime.startTime - workingTime.pauseTime - startPauseTime

    state.workingTimeFormat = formatSeconds(workingSeconds)
    if (!state.pauseWork) {
        workTimer = window.setInterval(() => {
            workingSeconds++
            state.workingTimeFormat = formatSeconds(workingSeconds)
        }, 1000)
    }
}


onActivated(() => {
    echartsResize()
})

onMounted(() => {
    startWork()
    // initCountUp() // 获取数据后再执行
    useEventListener(window, 'resize', echartsResize)
})

onBeforeMount(() => {
    for (const key in state.charts) {
        state.charts[key].dispose()
    }
})

onUnmounted(() => {
    clearInterval(workTimer)
})

watch(
    () => navTabs.state.tabFullScreen,
    () => {
        echartsResize()
    }
)

const formatSeconds = (seconds: number) => {
    var secondTime = 0 // 秒
    var minuteTime = 0 // 分
    var hourTime = 0 // 小时
    var dayTime = 0 // 天
    var result = ''

    if (seconds < 60) {
        secondTime = seconds
    } else {
        // 获取分钟，除以60取整数，得到整数分钟
        minuteTime = Math.floor(seconds / 60)
        // 获取秒数，秒数取佘，得到整数秒数
        secondTime = Math.floor(seconds % 60)
        // 如果分钟大于60，将分钟转换成小时
        if (minuteTime >= 60) {
            // 获取小时，获取分钟除以60，得到整数小时
            hourTime = Math.floor(minuteTime / 60)
            // 获取小时后取佘的分，获取分钟除以60取佘的分
            minuteTime = Math.floor(minuteTime % 60)
            if (hourTime >= 24) {
                // 获取天数， 获取小时除以24，得到整数天
                dayTime = Math.floor(hourTime / 24)
                // 获取小时后取余小时，获取分钟除以24取余的分；
                hourTime = Math.floor(hourTime % 24)
            }
        }
    }

    result =
        hourTime +
        t('dashboard.hour') +
        ((minuteTime >= 10 ? minuteTime : '0' + minuteTime) + t('dashboard.minute')) +
        ((secondTime >= 10 ? secondTime : '0' + secondTime) + t('dashboard.second'))
    if (dayTime > 0) {
        result = dayTime + t('dashboard.day') + result
    }
    return result
}
</script>

<style scoped lang="scss">
.welcome {
    background: #e1eaf9;
    border-radius: 6px;
    display: flex;
    align-items: center;
    padding: 15px 20px !important;
    box-shadow: 0 0 30px 0 rgba(82, 63, 105, 0.05);
    .welcome-img {
        height: 100px;
        margin-right: 10px;
        user-select: none;
    }
    .welcome-title {
        font-size: 1.5rem;
        line-height: 30px;
        color: var(--ba-color-primary-light);
    }
    .welcome-note {
        padding-top: 6px;
        font-size: 15px;
        color: var(--el-text-color-primary);
    }
}
.working {
    height: 130px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    height: 100%;
    position: relative;
    &:hover {
        .working-coffee {
            -webkit-transform: translateY(-4px) scale(1.02);
            -moz-transform: translateY(-4px) scale(1.02);
            -ms-transform: translateY(-4px) scale(1.02);
            -o-transform: translateY(-4px) scale(1.02);
            transform: translateY(-4px) scale(1.02);
            z-index: 999;
        }
    }
    .working-coffee {
        transition: all 0.3s ease;
        width: 80px;
    }
    .working-text {
        display: block;
        width: 100%;
        font-size: 15px;
        text-align: center;
        color: var(--el-text-color-primary);
    }
    .working-opt {
        position: absolute;
        top: -40px;
        right: 10px;
        background-color: rgba($color: #000000, $alpha: 0.3);
        padding: 10px 20px;
        border-radius: 20px;
        color: var(--ba-bg-color-overlay);
        transition: all 0.3s ease;
        cursor: pointer;
        opacity: 0;
        z-index: 999;
        &:active {
            background-color: rgba($color: #000000, $alpha: 0.6);
        }
    }
    &:hover {
        .working-opt {
            opacity: 1;
            top: 0;
        }
    }
}
.small-panel-box {
    margin-top: 20px;
}
.small-panel {
    background-color: #e9edf2;
    border-radius: var(--el-border-radius-base);
    padding: 25px;
    margin-bottom: 20px;
    .small-panel-title {
        color: #92969a;
        font-size: 15px;
    }
    .small-panel-content {
        display: flex;
        align-items: flex-end;
        margin-top: 20px;
        color: #2c3f5d;
        .content-left {
            font-size: 24px;
            // 这里不要删除
            .icon {
                margin-right: 10px;
            }
            span {
                display: inline-block;
                font-size: 28px;
            }
        }
    }
}

@media screen and (max-width: 425px) {
    .welcome-img {
        display: none;
    }
}
html.dark {
    .welcome {
        background-color: var(--ba-bg-color-overlay);
    }
    .small-panel {
        background-color: var(--ba-bg-color-overlay);
        .small-panel-content {
            color: var(--el-text-color-regular);
        }
    }
}
</style>
```

## 2.后端代码修改

```php
class Dashboard extends Backend
{
    // 获取首页数据 (好像return 是多余的?)
    public function getIndexData()
    {
        $this->success('', [
            'goods_num' => 300,
            'order_num' => 500,
            'stock_num' => 1000,
            'order_1' => 30,
            'order_2' => 150,
            'order_3' => 720,
        ]);
    }
}
```