<template>
  <ul class="videoList" :class="{ 'fullscreen-mode': isFullscreen }">
    <VideoListItem 
      @videoSelect="onVideoSelect" 
      v-for="video in videos" 
      :key="video.etag" 
      :video="video"
    />
  </ul>
</template>

<script>
import VideoListItem from './VideoListItem.vue'; 
export default {
    props: ['videos'],
    emits: ['videoSelect'],
    components: {
      VideoListItem
    },
    data() {
      return {
        isFullscreen: false
      }
    },
    methods: {
      onVideoSelect(video) {
        this.$emit('videoSelect', video);
        this.checkFullscreen();
      },
      checkFullscreen() {
        this.isFullscreen = !!document.fullscreenElement;
      }
    },
    mounted() {
      document.addEventListener('fullscreenchange', this.checkFullscreen);
    },
    beforeUnmount() {
      document.removeEventListener('fullscreenchange', this.checkFullscreen);
    }
}
</script>

<style scoped>
.videoList {
  list-style-type: none;
  width: 100%;
  transition: all 0.3s ease;
}

.videoList {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.videoList.fullscreen-mode {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 30%;
  max-height: 50vh;
  overflow-y: auto;
  background: rgba(0, 0, 0, 0.8);
  padding: 10px;
  z-index: 1000;
  border-radius: 10px 10px 0 0;
}

/* Mobil uyumluluk için */
@media (max-width: 768px) {
  .videoList.fullscreen-mode {
    width: 100%;
    max-height: 30vh;
  }
}
</style>