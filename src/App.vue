<template>
  <div class="container">
    <h1 class="title">Youtube App</h1>
    <SearchBar @termChange="onTermChange" />
    <div>
      <VideoDetail :video="selectedVideo"/>
      <VideoList @videoSelect="onVideoSelect" :videos="videos" />
    </div>

  </div>
</template>

<script>
import SearchBar from "@/components/SearchBar.vue";
import VideoList from "@/components/VideoList.vue";
import VideoDetail from "@/components/VideoDetail.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  data() {
    return {
      videos: [],
      selectedVideo: null,
    };
  },
  methods: {
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
    onTermChange(searchTerm) {
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            part: "snippet",
            type: "video",
            key: "AIzaSyAx7QoqzuL5YtCvmgRKKsXbKWb5Ib4SDEM",
            q: searchTerm,
          },
        })
        .then((response) => {
            console.log(response);
          this.videos = response.data.items;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.container {
  max-width: 1200px;
  width: 100%;
  margin: 50px auto;
}
.title {
  text-align: center;
}
</style>
