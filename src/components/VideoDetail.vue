<template>
  <div v-if="video" class="selectedVideoContainer" :class="{ 'fullscreen-mode': isFullscreen }">
    <div class="video-wrapper">
      <iframe 
        :src="videoUrl" 
        class="selectedVideo"
        ref="videoIframe"
        allowfullscreen
      ></iframe>
      <button @click="toggleFullscreen" class="fullscreen-button">
        {{ isFullscreen ? 'Exit Fullscreen' : '' }}
      </button>
    </div>
    <div class="details">
      <h4>{{ video.snippet.title }}</h4>
      <p>{{ video.snippet.description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['video'],
  data() {
    return {
      isFullscreen: false
    }
  },
  computed: {
    videoUrl() {
      const { videoId } = this.video.id;
      return `https://www.youtube.com/embed/${videoId}`;
    }
  },
  methods: {
    toggleFullscreen() {
      if (!this.isFullscreen) {
        this.enterFullscreen();
      } else {
        this.exitFullscreen();
      }
    },
    enterFullscreen() {
      const element = this.$el;
      
      if (element.requestFullscreen) {
        element.requestFullscreen();
      } else if (element.webkitRequestFullscreen) { 
        element.webkitRequestFullscreen();
      } else if (element.msRequestFullscreen) { 
        element.msRequestFullscreen();
      }
      
      this.isFullscreen = true;
      this.$emit('fullscreenChange', true);
    },
    exitFullscreen() {
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.webkitExitFullscreen) { 
        document.webkitExitFullscreen();
      } else if (document.msExitFullscreen) { 
        document.msExitFullscreen();
      }
      
      this.isFullscreen = false;
      this.$emit('fullscreenChange', false);
    },
    handleFullscreenChange() {
      this.isFullscreen = !!document.fullscreenElement;
      this.$emit('fullscreenChange', this.isFullscreen);
    }
  },
  mounted() {
    document.addEventListener('fullscreenchange', this.handleFullscreenChange);
    document.addEventListener('webkitfullscreenchange', this.handleFullscreenChange);
    document.addEventListener('msfullscreenchange', this.handleFullscreenChange);
  },
  beforeUnmount() {
    document.removeEventListener('fullscreenchange', this.handleFullscreenChange);
    document.removeEventListener('webkitfullscreenchange', this.handleFullscreenChange);
    document.removeEventListener('msfullscreenchange', this.handleFullscreenChange);
  }
}
</script>

<style scoped>
.selectedVideoContainer {
  width: 100%;
  position: relative;
}

.video-wrapper {
  position: relative;
}

.selectedVideo {
  width: 100%;
  height: 250px;
  border: none;
}

.details {
  border: 1px solid gray;
  border-radius: 4px;
  padding: 10px;
  margin-top: 10px;
}

.fullscreen-button {
  position: absolute;
  bottom: 10px;
  right: 10px;
  padding: 5px 10px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  z-index: 10;
}

.fullscreen-button:hover {
  background: rgba(0, 0, 0, 0.9);
}

.selectedVideoContainer.fullscreen-mode {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: black;
  z-index: 1000;
  display: flex;
  flex-direction: column;
}

.selectedVideoContainer.fullscreen-mode .video-wrapper {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.selectedVideoContainer.fullscreen-mode .selectedVideo {
  width: 100%;
  height: 100%;
  max-width: 100%;
  max-height: 100%;
}

.selectedVideoContainer.fullscreen-mode .details {
  background: black;
  color: white;
  border: none;
  padding: 15px;
  margin: 0;
}

@media (min-aspect-ratio: 16/9) {
  .selectedVideoContainer.fullscreen-mode .selectedVideo {
    width: 100%;
    height: auto;
  }
}

@media (max-aspect-ratio: 16/9) {
  .selectedVideoContainer.fullscreen-mode .selectedVideo {
    width: auto;
    height: 100%;
  }
}
</style>