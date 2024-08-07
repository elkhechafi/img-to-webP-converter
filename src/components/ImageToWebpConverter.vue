<template>
  <div class="image-to-webp-converter" @dragover.prevent @drop.prevent="onDrop">
    <div
      class="drop-zone"
      :class="{ 'drop-zone--over': isDragging }"
      @dragenter.prevent="isDragging = true"
      @dragleave.prevent="isDragging = false"
    >
      <input
        type="file"
        @change="onFileChange"
        accept="image/*"
        id="fileInput"
        class="file-input"
      />
      <label for="fileInput" class="file-label">
        <span v-if="!selectedFile">Choose a file or drag it here</span>
        <span v-else>{{ selectedFile.name }}</span>
      </label>
    </div>

    <button @click="convertToWebP" :disabled="!selectedFile" class="convert-button">
      Convert to WebP
    </button>

    <div v-if="convertedImage" class="result">
      <img :src="convertedImage" alt="Converted image" class="preview-image" />
      <a :href="convertedImage" :download="webpFileName" class="download-button"> Download WebP </a>
    </div>
  </div>
</template>

<script>
import imageCompression from 'browser-image-compression'

export default {
  name: 'ImageToWebpConverter',
  data() {
    return {
      selectedFile: null,
      convertedImage: null,
      webpFileName: '',
      isDragging: false
    }
  },
  methods: {
    onFileChange(e) {
      this.handleFile(e.target.files[0])
    },
    onDrop(e) {
      this.isDragging = false
      this.handleFile(e.dataTransfer.files[0])
    },
    handleFile(file) {
      if (file && file.type.startsWith('image/')) {
        this.selectedFile = file
        this.webpFileName = this.getWebPFileName(file.name)
      } else {
        alert('Please select a valid image file.')
      }
    },
    getWebPFileName(originalName) {
      const nameWithoutExtension = originalName.split('.').slice(0, -1).join('.')
      return `${nameWithoutExtension}.webp`
    },
    async convertToWebP() {
      if (!this.selectedFile) return

      try {
        const options = {
          maxSizeMB: 1,
          maxWidthOrHeight: 1920,
          useWebWorker: true,
          fileType: 'image/webp'
        }

        const compressedFile = await imageCompression(this.selectedFile, options)

        const convertedFile = new File([compressedFile], this.webpFileName, {
          type: 'image/webp',
          lastModified: new Date().getTime()
        })

        this.convertedImage = URL.createObjectURL(convertedFile)
        this.$emit('conversion-complete', this.convertedImage, convertedFile)
      } catch (error) {
        console.error('Error converting image:', error)
        this.$emit('conversion-error', error)
      }
    }
  }
}
</script>

<style scoped>
.image-to-webp-converter {
  font-family: Arial, sans-serif;
  display: flex;
  width: 100%;
  margin: 0 auto;
  padding: 20px;
}

.drop-zone {
  width: 50vw;
  min-height: 5vh;
  border: 2px dashed #ccc;
  border-radius: 4px;
  padding: 20px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.drop-zone--over {
  border-color: #000;
  background-color: #f0f0f0;
}

.file-input {
  display: none;
}

.file-label {
  display: block;
  font-size: 16px;
  color: #555;
}

.convert-button,
.download-button {
  display: block;
  width: 60%;
  padding: 10px;
  margin: 10px 20%;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.convert-button:hover,
.download-button:hover {
  background-color: #0056b3;
}

.convert-button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.result {
  margin-top: 20px;
}

.preview-image {
  max-width: 100%;
  margin-bottom: 10px;
}

.download-button {
  text-decoration: none;
  text-align: center;
}
</style>
