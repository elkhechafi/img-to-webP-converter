<template>
  <div id="app">
    <h1 class="main-title">Image to WebP converter</h1>
    <ImageToWebpConverter
      @conversion-complete="handleConversionComplete"
      @conversion-error="handleConversionError"
    />
    <!-- Other components and content -->
  </div>

  <div class="content">
    <h2 class="advantages-title">WebP Advantages</h2>
    <ol class="list-advantages">
      <li>
        <p class="list-mainTitle">Smaller File Size:</p>
        <ul>
          <li>
            Lossy Compression: WebP images with lossy compression are typically 25-34% smaller than
            comparable JPEG images.
          </li>
          <li>
            Lossless Compression: WebP images with lossless compression can be up to 26% smaller
            than comparable PNG images.
          </li>
          <li>
            Smaller file sizes lead to faster loading times, which improves the user experience and
            reduces bandwidth usage.
          </li>
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Quality Preservation:</p>
        <ul>
          <li>
            WebP supports both lossy and lossless compression, allowing you to choose the best
            balance between image quality and file size.
          </li>
          <li>The format can maintain high visual quality even at smaller sizes.</li>
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Transparency</p>
        <ul>
          <li>
            WebP supports transparency (alpha channel) in both lossy and lossless modes, similar to
            PNG, but with potentially smaller file sizes.
          </li>
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Animation</p>
        <ul>
          WebP supports animated images, similar to GIFs, but with better compression, resulting in
          smaller file sizes.
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Wide Browser Support:</p>
        <ul>
          <li>
            Most modern web browsers, including Chrome, Firefox, Edge, and Opera, support WebP
            natively.
          </li>
          <li>Safari added support for WebP starting with version 14.</li>
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Progressive Decoding:</p>
        <ul>
          <li>
            WebP supports progressive decoding, allowing images to be displayed incrementally as
            they are downloaded, which can improve the perceived loading speed.
          </li>
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Improved Performance:</p>
        <ul>
          Using WebP can contribute to faster website performance, which can positively impact SEO
          rankings and user engagement.
        </ul>
      </li>
      <li>
        <p class="list-mainTitle">Reduced Storage Costs:</p>
        <ul>
          <li>
            Smaller image sizes mean reduced storage requirements on servers and potentially lower
            costs for cloud storage and content delivery networks (CDNs).
          </li>
        </ul>
      </li>
    </ol>
  </div>
</template>

<script>
import ImageToWebpConverter from './components/ImageToWebpConverter.vue'

export default {
  name: 'App',
  components: {
    ImageToWebpConverter
  },
  methods: {
    handleConversionComplete(convertedImageUrl, convertedFile) {
      console.log('Image converted successfully:', convertedImageUrl)
      console.log('Converted file:', convertedFile)
      // Do something with the converted image URL or file
    },
    handleConversionError(error) {
      console.error('Error during conversion:', error)
      // Handle the error (e.g., show an error message to the user)
    }
  }
}
</script>

<style scoped>
.main-title {
  width: 100%;
  text-align: center;
}

.content {
  margin-top: 3vh;
  width: 100%;
  text-align: center;
}

.list-advantages {
  text-align: left;
}

.advantages-title {
  font-size: 4rem;
}
.list-mainTitle {
  font-size: 1.5rem;
}
</style>
