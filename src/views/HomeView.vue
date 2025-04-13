<template>
  <div class="bg-gray-50 min-h-screen">
    <main class="pt-16 max-w-[1440px] mx-auto px-8">
      <div class="grid grid-cols-7 gap-6 py-6">
        <div class="col-span-5 space-y-6">
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-xl font-medium">实时卫星监测</h2>
              <div class="flex gap-4">
                <button
                  class="px-3 py-1.5 border border-gray-200 !rounded-button hover:border-secondary whitespace-nowrap"
                >
                  <i class="fas fa-layer-group mr-2"></i>图层
                </button>
                <button
                  class="px-3 py-1.5 border border-gray-200 !rounded-button hover:border-secondary whitespace-nowrap"
                >
                  <i class="fas fa-expand mr-2"></i>全屏
                </button>
              </div>
            </div>
            <div class="h-[480px] rounded-lg relative overflow-hidden">
              <div id="container" class="absolute inset-0 w-full h-full">
                <div class="absolute bottom-4 left-4 bg-white p-4 rounded-lg shadow-lg z-10">
                  <div class="flex items-center gap-4 mb-4">
                    <button
                      class="w-8 h-8 flex items-center justify-center bg-secondary text-white rounded-full"
                    >
                      <i class="fas fa-play"></i>
                    </button>
                    <div class="flex items-center gap-2">
                      <span class="text-sm text-gray-600">2025-04-01</span>
                      <input
                        type="range"
                        class="w-48 h-1 bg-gray-200 rounded-lg appearance-none cursor-pointer slider-thumb"
                      />
                      <span class="text-sm text-gray-600">2025-05-01</span>
                    </div>
                  </div>
                  <div class="flex gap-4">
                    <div class="flex items-center gap-2">
                      <span class="w-3 h-3 rounded-full bg-red-500"></span>
                      <span class="text-sm">高密度区域</span>
                    </div>
                    <div class="flex items-center gap-2">
                      <span class="w-3 h-3 rounded-full bg-yellow-500"></span>
                      <span class="text-sm">中密度区域</span>
                    </div>
                    <div class="flex items-center gap-2">
                      <span class="w-3 h-3 rounded-full bg-green-500"></span>
                      <span class="text-sm">低密度区域</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <div class="flex justify-between items-center mb-6">
              <h2 class="text-xl font-medium">预测分析</h2>
              <div class="flex gap-4">
                <button
                  class="px-3 py-1.5 text-sm bg-gray-50 hover:bg-gray-100 !rounded-button whitespace-nowrap"
                >
                  <i class="fas fa-calendar-alt mr-2"></i>最近 7 天
                </button>
                <button
                  class="px-3 py-1.5 text-sm bg-gray-50 hover:bg-gray-100 !rounded-button whitespace-nowrap"
                >
                  <i class="fas fa-download mr-2"></i>导出数据
                </button>
              </div>
            </div>
            <div class="grid grid-cols-2 gap-6">
              <div>
                <div class="flex items-center justify-between mb-4">
                  <div class="text-sm text-gray-600">垃圾分布密度趋势</div>
                  <div class="flex gap-2">
                    <button
                      class="w-6 h-6 flex items-center justify-center text-xs border border-secondary bg-opacity-10 text-secondary !rounded-button"
                    >
                      日
                    </button>
                    <button
                      class="w-6 h-6 flex items-center justify-center text-xs text-gray-400 hover:bg-gray-100 !rounded-button"
                    >
                      周
                    </button>
                    <button
                      class="w-6 h-6 flex items-center justify-center text-xs text-gray-400 hover:bg-gray-100 !rounded-button"
                    >
                      月
                    </button>
                  </div>
                </div>
                <div ref="chart1Ref" class="h-[240px]"></div>
                <div class="flex items-center justify-between mt-4 text-sm">
                  <div class="flex items-center gap-2">
                    <i class="fas fa-arrow-trend-up text-green-500"></i>
                    <span>同比增长 12.5%</span>
                  </div>
                  <div class="flex items-center gap-2">
                    <i class="fas fa-chart-line text-blue-500"></i>
                    <span>峰值出现在周五</span>
                  </div>
                </div>
              </div>
              <div>
                <div class="flex items-center justify-between mb-4">
                  <div class="text-sm text-gray-600">垃圾类型分布</div>
                  <div class="flex gap-2">
                    <button
                      class="px-2 py-1 text-xs border border-secondary text-secondary !rounded-button"
                    >
                      数量
                    </button>
                    <button
                      class="px-2 py-1 text-xs text-gray-400 hover:bg-gray-100 !rounded-button"
                    >
                      体积
                    </button>
                  </div>
                </div>
                <div ref="chart2Ref" class="h-[240px]"></div>
                <div class="flex items-center justify-between mt-4 text-sm">
                  <div class="flex items-center gap-2">
                    <i class="fas fa-circle text-primary"></i>
                    <span>塑料占比最高</span>
                  </div>
                  <div class="flex items-center gap-2">
                    <i class="fas fa-circle text-secondary"></i>
                    <span>渔网数量增加</span>
                  </div>
                </div>
              </div>
            </div>
            <div class="grid grid-cols-4 gap-4 mt-6">
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">今日新增</div>
                <div class="text-2xl font-medium">284</div>
                <div class="text-xs text-green-500 mt-1">↑ 8.5%</div>
              </div>
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">平均密度</div>
                <div class="text-2xl font-medium">46.2</div>
                <div class="text-xs text-red-500 mt-1">↓ 3.2%</div>
              </div>
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">影响范围</div>
                <div class="text-2xl font-medium">892</div>
                <div class="text-xs text-green-500 mt-1">↑ 12.8%</div>
              </div>
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">预警等级</div>
                <div class="text-2xl font-medium text-orange-500">中度</div>
                <div class="text-xs text-gray-400 mt-1">持续监测中</div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-span-2 space-y-6">
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <h2 class="text-xl font-medium mb-4">实时数据</h2>
            <div class="space-y-4">
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">检测区域</div>
                <div class="text-2xl font-medium">东海海域</div>
              </div>
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">垃圾覆盖面积</div>
                <div class="text-2xl font-medium">2,458 km²</div>
              </div>
              <div class="p-4 bg-gray-50 rounded-lg">
                <div class="text-sm text-gray-600 mb-1">预计清理时间</div>
                <div class="text-2xl font-medium">约 72 小时</div>
              </div>
            </div>
          </div>
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <h2 class="text-xl font-medium mb-4">行动计划</h2>
            <div class="space-y-4">
              <button
                class="w-full bg-secondary text-white py-3 !rounded-button hover:bg-opacity-90 whitespace-nowrap"
              >
                <i class="fas fa-users mr-2"></i>参与志愿行动
              </button>
              <button
                class="w-full border border-secondary text-secondary py-3 !rounded-button hover:bg-gray-50 whitespace-nowrap"
              >
                <i class="fas fa-file-download mr-2"></i>下载详细报告
              </button>
            </div>
          </div>
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <h2 class="text-xl font-medium mb-4">最新动态</h2>
            <div class="space-y-4">
              <div class="flex items-start gap-4">
                <div
                  class="w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center flex-shrink-0"
                >
                  <i class="fas fa-satellite text-blue-500"></i>
                </div>
                <div>
                  <div class="text-sm font-medium">卫星数据更新</div>
                  <div class="text-sm text-gray-600">检测到新的垃圾聚集区域，位于东经 123.45°</div>
                  <div class="text-xs text-gray-400 mt-1">10 分钟前</div>
                </div>
              </div>
              <div class="flex items-start gap-4">
                <div
                  class="w-10 h-10 rounded-full bg-green-100 flex items-center justify-center flex-shrink-0"
                >
                  <i class="fas fa-check text-green-500"></i>
                </div>
                <div>
                  <div class="text-sm font-medium">清理任务完成</div>
                  <div class="text-sm text-gray-600">志愿者团队已完成南海区域的清理工作</div>
                  <div class="text-xs text-gray-400 mt-1">1 小时前</div>
                </div>
              </div>
              <div class="flex items-start gap-4">
                <div
                  class="w-10 h-10 rounded-full bg-yellow-100 flex items-center justify-center flex-shrink-0"
                >
                  <i class="fas fa-exclamation-triangle text-yellow-500"></i>
                </div>
                <div>
                  <div class="text-sm font-medium">预警提醒</div>
                  <div class="text-sm text-gray-600">预测 48 小时内将有大量垃圾漂向沿海地区</div>
                  <div class="text-xs text-gray-400 mt-1">2 小时前</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue'
import * as echarts from 'echarts'
import AMapLoader from '@amap/amap-jsapi-loader'

// 地图实例
const mapInstance = ref(null)

// Refs for chart containers
const chart1Ref = ref(null)
const chart2Ref = ref(null)

// ECharts instances
let chart1Instance = null
let chart2Instance = null

// Chart options
const option1 = {
  animation: false,
  tooltip: {
    trigger: 'axis',
  },
  grid: {
    top: '10px',
    left: '40px',
    right: '20px',
    bottom: '20px',
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: ['12-20', '12-21', '12-22', '12-23', '12-24', '12-25', '12-26'],
    axisLine: {
      lineStyle: {
        color: '#E5E7EB',
      },
    },
    axisTick: {
      show: false,
    },
    axisLabel: {
      color: '#6B7280', // Added for better visibility if needed
    },
  },
  yAxis: {
    type: 'value',
    splitLine: {
      lineStyle: {
        color: '#E5E7EB',
      },
    },
    axisLine: {
      show: false,
    },
    axisTick: {
      show: false,
    },
    axisLabel: {
      color: '#6B7280', // Added for better visibility if needed
    },
  },
  series: [
    {
      data: [820, 932, 901, 934, 1290, 1330, 1320],
      type: 'line',
      smooth: true,
      symbolSize: 8,
      lineStyle: {
        width: 3,
        color: '#17A2B8', // secondary color
      },
      itemStyle: {
        color: '#17A2B8', // secondary color
      },
      areaStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [
            {
              offset: 0,
              color: 'rgba(23, 162, 184, 0.2)', // secondary color with opacity
            },
            {
              offset: 1,
              color: 'rgba(23, 162, 184, 0)', // secondary color transparent
            },
          ],
        },
      },
    },
  ],
}

const option2 = {
  animation: false,
  tooltip: {
    trigger: 'item',
    formatter: '{b}: {d}%',
  },
  legend: {
    bottom: '0%',
    left: 'center',
    itemWidth: 12,
    itemHeight: 12,
    textStyle: {
      color: '#666',
      fontSize: 12,
    },
  },
  series: [
    {
      type: 'pie',
      radius: ['40%', '70%'],
      center: ['50%', '40%'], // Adjusted center slightly for better legend spacing
      avoidLabelOverlap: false,
      itemStyle: {
        borderColor: '#fff',
        borderWidth: 2,
      },
      label: {
        show: false,
      },
      labelLine: {
        show: false,
      },
      data: [
        { value: 1048, name: '塑料', itemStyle: { color: '#1E3D59' } }, // primary color
        { value: 735, name: '渔网', itemStyle: { color: '#17A2B8' } }, // secondary color
        { value: 580, name: '泡沫', itemStyle: { color: '#FF6B6B' } },
        { value: 484, name: '其他', itemStyle: { color: '#4ECDC4' } },
      ],
    },
  ],
}

// Function to initialize charts
const initCharts = () => {
  if (chart1Ref.value) {
    chart1Instance = echarts.init(chart1Ref.value)
    chart1Instance.setOption(option1)
  }
  if (chart2Ref.value) {
    chart2Instance = echarts.init(chart2Ref.value)
    chart2Instance.setOption(option2)
  }
}

// Function to resize charts
const resizeCharts = () => {
  chart1Instance?.resize()
  chart2Instance?.resize()
}

// 初始化地图
const initMap = async () => {
  window._AMapSecurityConfig = {
    securityJsCode: import.meta.env.VITE_AMAP_SECURITY_JS_CODE,
  }
  const AMap = await AMapLoader.load({
    key: import.meta.env.VITE_AMAP_KEY,
    version: '2.0',
    plugins: ['AMap.Scale', 'AMap.ToolBar'],
  })

  mapInstance.value = new AMap.Map('container', {
    zoom: 9,
    center: [122.2030363, 29.9873344],
    pitch: 0,
    rotation: 0,
    viewMode: '3D',
    skyColor: '#1E90FF', // 天空颜色
    features: ['bg', 'building', 'point'], // 只显示必要的图层
    showBuildingBlock: true, // 显示3D建筑
  })

  // 添加控件
  mapInstance.value.addControl(new AMap.Scale())
  mapInstance.value.addControl(new AMap.ToolBar())
}

// Mount lifecycle hook
onMounted(async () => {
  // 初始化地图
  await initMap()
  // Ensure DOM is ready before initializing charts
  await nextTick()
  initCharts()
  // Add resize listener
  window.addEventListener('resize', resizeCharts)
})

// Before unmount lifecycle hook
onBeforeUnmount(() => {
  // Remove resize listener
  window.removeEventListener('resize', resizeCharts)
  // Dispose ECharts instances
  chart1Instance?.dispose()
  chart2Instance?.dispose()
  // 销毁地图实例
  mapInstance.value?.destroy()
})
</script>

<style scoped>
/* Using Tailwind utility classes directly is preferred, but adding specific styles here if needed */

/* Custom slider thumb style */
.slider-thumb::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 16px; /* 1rem */
  height: 16px; /* 1rem */
  background: #17a2b8; /* secondary color */
  border-radius: 9999px; /* rounded-full */
  cursor: pointer;
}

.slider-thumb::-moz-range-thumb {
  width: 16px;
  height: 16px;
  background: #17a2b8;
  border-radius: 50%;
  cursor: pointer;
  border: none; /* Needed for Firefox */
}

/* Apply Tailwind's theme colors and border-radius */
.text-primary {
  color: theme('colors.primary');
}
.text-secondary {
  color: theme('colors.secondary');
}
.bg-primary {
  background-color: theme('colors.primary');
}
.bg-secondary {
  background-color: theme('colors.secondary');
}
.border-secondary {
  border-color: theme('colors.secondary');
}
.\!rounded-button {
  border-radius: 0.5rem !important; /* 使用具体值替代 theme 函数 */
}
</style>

<style>
/* 高德地图容器样式 */
.amap-container {
  width: 100%;
  height: 100%;
}

/* 高德地图控件样式调整 */
:deep(.amap-scale) {
  left: 20px !important;
  bottom: 100px !important;
}

:deep(.amap-toolbar) {
  right: 20px !important;
  top: 20px !important;
}

:deep(.amap-control) {
  z-index: 5;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 4px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}
</style>
