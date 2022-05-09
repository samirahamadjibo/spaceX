<template>
  <div class="popup-wrapper">
    <div class="popup-body">
      <iframe
        v-if="!isClosed"
        width="560"
        height="315"
        v-bind:src="getLink()"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        class="yvideo"
      ></iframe>
      <button type="button" @click="close">close video</button>
    </div>
  </div>
</template>
  
<script>
export default {
  name: "PopUp",
  props: {
    youtubeId: {
      type: String,
    },
  },
  data() {
    return {
      isClosed: false,
    };
  },
  methods: {
    getLink() {
      return "https://www.youtube.com/embed/" + this.youtubeId;
    },
    closeVideo() {
      isClosed = true;
    },
    close() {
      this.$emit("close");
    },
  },
};

// // 2. This code loads the IFrame Player API code asynchronously.
// var tag = document.createElement("script");

// tag.src = "https://www.youtube.com/iframe_api";
// var firstScriptTag = document.getElementsByTagName("script")[0];
// firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

// // 3. This function creates an <iframe> (and YouTube player)
// //    after the API code downloads.
// var player;
// function onYouTubeIframeAPIReady() {
//   player = new YT.Player("player", {
//     height: "360",
//     width: "640",
//     videoId: "M7lc1UVf-VE",
//     events: {
//       onReady: onPlayerReady,
//       onStateChange: onPlayerStateChange,
//     },
//   });
// }

// // 4. The API will call this function when the video player is ready.
// function onPlayerReady(event) {
//   event.target.playVideo();
// }

// // 5. The API calls this function when the player's state changes.
// //    The function indicates that when playing a video (state=1),
// //    the player should play for six seconds and then stop.
// var done = false;
// function onPlayerStateChange(event) {
//   if (event.data == YT.PlayerState.PLAYING && !done) {
//     setTimeout(stopVideo, 6000);
//     done = true;
//   }
// }
// function stopVideo() {
//   player.stopVideo();
// }
</script>

<style>
.popup-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.13);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-body {
  background: white;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 30px 30px;
  min-width: 600px;
  min-height: 400px;
}
</style>