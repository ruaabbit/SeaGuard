<template>
  <div class="min-h-[1024px] bg-gray-50">
    <div class="max-w-[1440px] mx-auto px-8 py-12">
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
            </div>
            <div class="flex justify-between items-center">
              <div class="text-sm text-gray-500">
                <p>
                  图片尺寸：<span>{{ originalSize }}</span>
                </p>
                <p>
                  上传时间：<span>{{ uploadTime }}</span>
                </p>
                <p>
                  检测完成时间：<span>{{ processTime }}</span>
                </p>
                <p>
                  识别垃圾数量：<span>{{ detectedCount }}</span>
                </p>
              </div>
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
const uploadAreaRef = ref(null) // Optional: if needed for direct manipulation

const isDragging = ref(false)
const showProgress = ref(false)
const showResult = ref(false)
const progressValue = ref(0)
const progressText = ref('0%')

// --- Result Data ---
// Initialize with placeholder/default values or null
const originalImageUrl = ref(null) // Or placeholder: 'https://image-resource.mastergo.com/...'
const resultImageUrl = ref(null) // Or placeholder: 'https://image-resource.mastergo.com/...'
const originalSize = ref('N/A')
const uploadTime = ref('N/A')
const processTime = ref('N/A')
const detectedCount = ref(0)
// --- End Result Data ---

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
    // Basic validation (optional)
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
    // Basic validation (optional)
    if (files[0].size > 10 * 1024 * 1024) {
      alert('文件大小不能超过 10MB')
      // Reset file input to allow re-selection of the same file if needed after error
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

  // --- Simulate Upload & Processing ---
  // In a real app:
  // 1. Use FileReader to display the original image preview immediately.
  // 2. Upload the file to a server using fetch or axios.
  // 3. Update progress based on upload events (if available) or server polling.
  // 4. On successful upload and processing, get results (processed image URL, data) from the server.

  const reader = new FileReader()
  reader.onload = (e) => {
    // Set original image preview (using the actual uploaded file)
    // originalImageUrl.value = e.target?.result; // Use this in a real scenario

    // Using placeholder for now as per original HTML
    originalImageUrl.value =
      'https://image-resource.mastergo.com/157116089141712/157116089141714/9b5b863fc90565557fd9f3d4533886df.png'

    // Get image dimensions (example - requires image to be loaded)
    const img = new Image()
    img.onload = () => {
      originalSize.value = `${img.naturalWidth} x ${img.naturalHeight}`
    }
    img.src = e.target?.result // Use the actual file data URL

    // Set upload time
    uploadTime.value = new Date().toLocaleString() // Use current time

    // Start simulation after reader loads
    simulateProgressAndProcessing(file)
  }
  reader.onerror = (error) => {
    console.error('FileReader error:', error)
    alert('读取文件时出错')
    showProgress.value = false
    if (fileInputRef.value) fileInputRef.value.value = '' // Reset input
  }
  reader.readAsDataURL(file) // Read the file to get Data URL
}

const simulateProgressAndProcessing = (/* file */) => {
  // In real app, this function would likely involve an API call
  // For now, just simulate progress
  let progress = 0
  const interval = setInterval(() => {
    progress += Math.random() * 10 + 5 // Simulate variable progress speed
    if (progress >= 100) {
      progress = 100
      clearInterval(interval)

      // Simulate API call completion
      setTimeout(() => {
        showProgress.value = false
        showResult.value = true

        // --- Set results (replace with actual API response) ---
        resultImageUrl.value =
          'https://image-resource.mastergo.com/157116089141712/157116089141714/dfe51ec005a7c36ec18818e022d10bc0.png' // Placeholder
        processTime.value = new Date().toLocaleString() // Use current time
        detectedCount.value = 8 // Placeholder
        // --- End Set results ---

        // Reset file input after successful processing
        if (fileInputRef.value) fileInputRef.value.value = ''
      }, 500) // Simulate network delay for result
    }
    progressValue.value = progress
    progressText.value = `${Math.round(progress)}%`
  }, 150) // Update progress interval
}

const downloadResult = () => {
  // In a real app, you might download the result image or a report.
  // This is a placeholder function.
  if (resultImageUrl.value) {
    // Example: Trigger download of the result image
    const link = document.createElement('a')
    link.href = resultImageUrl.value
    // You might want to fetch the image as a blob first to set a proper filename
    link.download = 'detection_result.png' // Or generate a dynamic name
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
  } else {
    alert('没有可下载的结果。')
  }
}
</script>

<style scoped>
/* Replicating inline styles from the original HTML */
.progress::-webkit-progress-bar {
  background-color: #f0f0f0;
  border-radius: 4px; /* Use theme's default or a specific value */
}

.progress::-webkit-progress-value {
  background-color: #0066cc; /* Use theme's primary color */
  border-radius: 4px; /* Use theme's default or a specific value */
  transition: width 0.3s ease; /* Add smooth transition */
}

/* Style for dragover state */
.upload-area.dragover {
  border-color: #0066cc; /* Use theme's primary color */
  background-color: rgba(0, 102, 204, 0.05); /* Lighter background on dragover */
}

/* Ensure Font Awesome icons work if loaded globally */
/* No specific styles needed here if FA is loaded correctly */

/* Apply custom button radius if defined in tailwind.config.js */
/* If 'rounded-button' is defined like: theme.extend.borderRadius.button = '4px' */
/* Tailwind should apply it automatically. If not, define it here: */
/*
.rounded-button {
  border-radius: 4px;
}
*/

/* Hide default file input */
/* input[type="file"] { display: none; } */ /* Already handled by 'hidden' class */
</style>
