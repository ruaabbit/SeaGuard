<template>
  <div class="min-h-[1024px] bg-gray-50">
    <div class="max-w-[1440px] mx-auto px-8 pt-24 pb-12">
      <div class="text-center mb-12">
        <h1 class="text-4xl font-bold text-gray-900 mb-4">海洋垃圾智能检测系统</h1>
        <p class="text-lg text-gray-600">上传图片，快速识别海洋垃圾</p>
      </div>

      <div class="max-w-3xl mx-auto mb-16">
        <!-- Upload Area -->
        <div
          ref="uploadAreaRef"
          @click="triggerFileInput"
          @dragover.prevent="handleDragOver"
          @dragleave.prevent="handleDragLeave"
          @drop.prevent="handleDrop"
          :class="[
            'upload-area border-2 border-dashed border-gray-300 rounded-lg p-12 text-center bg-white cursor-pointer transition-all hover:border-primary',
            { dragover: isDragging },
          ]"
        >
          <i class="fas fa-cloud-upload-alt text-5xl text-gray-400 mb-4"></i>
          <h3 class="text-xl font-medium text-gray-700 mb-2">拖拽文件到这里或点击上传</h3>
          <p class="text-gray-500 mb-4">支持 jpg、png 格式，单个文件大小不超过 10MB</p>
          <button
            class="bg-primary text-white px-6 py-2 rounded-button hover:bg-opacity-90 transition-all whitespace-nowrap"
            @click.stop="triggerFileInput"
          >
            <i class="fas fa-plus mr-2"></i>选择文件
          </button>
          <input
            type="file"
            ref="fileInputRef"
            @change="handleFileChange"
            accept=".jpg,.png"
            class="hidden"
          />
        </div>

        <!-- Progress Area -->
        <div v-if="showProgress" class="mt-8">
          <div class="flex items-center mb-2">
            <span class="text-sm text-gray-600 mr-4">处理中...</span>
            <span class="text-sm text-primary">{{ progressText }}</span>
          </div>
          <progress class="progress w-full h-2" :value="progressValue" max="100"></progress>
        </div>
      </div>

      <!-- Result Area -->
      <div v-if="showResult">
        <div class="max-w-3xl mx-auto mb-8">
          <div class="bg-white p-6 rounded-lg shadow-sm">
            <h3 class="text-lg font-medium text-gray-800 mb-4">检测结果</h3>
            <div class="aspect-video bg-gray-100 rounded-lg overflow-hidden mb-4 relative">
              <img
                v-if="originalImageUrl"
                :src="originalImageUrl"
                class="w-full h-full object-contain absolute top-0 left-0"
                alt="原始图片"
              />
              <img
                v-if="resultImageUrl"
                :src="resultImageUrl"
                class="w-full h-full object-contain absolute top-0 left-0 opacity-60"
                alt="检测结果"
              />
              <img
                src="/predict/legend_chinese.svg"
                class="absolute top-4 right-4 w-24 h-auto bg-white/80 rounded shadow-sm"
                alt="图例"
              />
            </div>
            <div class="flex justify-center items-center">
              <button
                class="bg-primary text-white px-4 py-2 rounded-button hover:bg-opacity-90 transition-all whitespace-nowrap"
                @click="downloadResult"
              >
                <i class="fas fa-download mr-2"></i>下载结果
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const fileInputRef = ref(null)
const uploadAreaRef = ref(null)

const isDragging = ref(false)
const showProgress = ref(false)
const showResult = ref(false)
const progressValue = ref(0)
const progressText = ref('0%')

const originalImageUrl = ref(null)
const resultImageUrl = ref(null)
const originalSize = ref('N/A')
const uploadTime = ref('N/A')
const processTime = ref('N/A')
const detectedCount = ref(0)

const triggerFileInput = () => {
  fileInputRef.value?.click()
}

const handleDragOver = () => {
  isDragging.value = true
}

const handleDragLeave = () => {
  isDragging.value = false
}

const handleDrop = (e) => {
  isDragging.value = false
  const files = e.dataTransfer?.files
  if (files && files.length > 0) {
    if (files[0].size > 10 * 1024 * 1024) {
      alert('文件大小不能超过 10MB')
      return
    }
    if (!['image/jpeg', 'image/png'].includes(files[0].type)) {
      alert('仅支持 jpg 和 png 格式')
      return
    }
    handleFile(files[0])
  }
}

const handleFileChange = (e) => {
  const files = e.target?.files
  if (files && files.length > 0) {
    if (files[0].size > 10 * 1024 * 1024) {
      alert('文件大小不能超过 10MB')
      if (fileInputRef.value) fileInputRef.value.value = ''
      return
    }
    if (!['image/jpeg', 'image/png'].includes(files[0].type)) {
      alert('仅支持 jpg 和 png 格式')
      if (fileInputRef.value) fileInputRef.value.value = ''
      return
    }
    handleFile(files[0])
  }
}

const handleFile = (file) => {
  showProgress.value = true
  showResult.value = false
  progressValue.value = 0
  progressText.value = '0%'

  // Reset previous results
  originalImageUrl.value = null
  resultImageUrl.value = null
  originalSize.value = 'N/A'
  uploadTime.value = 'N/A'
  processTime.value = 'N/A'
  detectedCount.value = 0

  const reader = new FileReader()
  reader.onload = (e) => {
    // 设置原始图片预览
    originalImageUrl.value = e.target?.result

    // 获取图片尺寸
    const img = new Image()
    img.onload = () => {
      originalSize.value = `${img.naturalWidth} x ${img.naturalHeight}`
    }
    img.src = e.target?.result

    // 设置上传时间
    uploadTime.value = new Date().toLocaleString()

    // 开始模拟处理过程
    simulateProgressAndProcessing(file)
  }
  reader.onerror = (error) => {
    console.error('FileReader error:', error)
    alert('读取文件时出错')
    showProgress.value = false
    if (fileInputRef.value) fileInputRef.value.value = ''
  }
  reader.readAsDataURL(file)
}

const simulateProgressAndProcessing = (file) => {
  let progress = 0
  const interval = setInterval(() => {
    progress += Math.random() * 10 + 5
    if (progress >= 100) {
      progress = 100
      clearInterval(interval)

      setTimeout(() => {
        showProgress.value = false
        showResult.value = true

        // 根据文件名规则选择结果图片
        const fileName = file.name
        const fileNameWithoutExt = fileName.substring(0, fileName.lastIndexOf('.'))
        const matchesPattern =
          fileNameWithoutExt.startsWith('Scene_1') && fileNameWithoutExt.endsWith('_34')

        // 根据匹配结果设置不同的结果图片
        resultImageUrl.value = matchesPattern
          ? '/predict/Scene_1_34_viz.png'
          : '/predict/Scene_71_8_viz.png'

        processTime.value = new Date().toLocaleString()
        detectedCount.value = 8

        if (fileInputRef.value) fileInputRef.value.value = ''
      }, 500)
    }
    progressValue.value = progress
    progressText.value = `${Math.round(progress)}%`
  }, 150)
}

const downloadResult = () => {
  if (resultImageUrl.value) {
    const link = document.createElement('a')
    link.href = resultImageUrl.value
    link.download = 'detection_result.png'
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
  } else {
    alert('没有可下载的结果。')
  }
}
</script>

<style scoped>
.progress::-webkit-progress-bar {
  background-color: #f0f0f0;
  border-radius: 4px;
}

.progress::-webkit-progress-value {
  background-color: #0066cc;
  border-radius: 4px;
  transition: width 0.3s ease;
}

.upload-area.dragover {
  border-color: #0066cc;
  background-color: rgba(0, 102, 204, 0.05);
}
</style>
