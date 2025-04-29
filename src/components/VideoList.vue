<template>
  <ul class="videoList">
    <VideoListItem @video-select="onVideoSelect" v-for=" video in videos" :key="video.etag" :video="video"/>
  </ul>
</template>

<script>
import VideoListItem from './VideoListItem.vue'; 
export default {
    props:['videos'],
    emits:['videoSelect'],
    components:{
      VideoListItem
    },
    methods: {
      onVideoSelect(video){
        this.$emit('videoSelect',video)
      }
    },
    
}
</script>

<style scoped>
.videoList {
  list-style-type: none;

}

</style>    