<template>
  <section class="play-page">
    <div
      class="mask"
      :style="{
        backgroundImage: `url(${
          currentSong.song ? currentSong.picUrl : currentSong.al.picUrl
        }?imageView=1&type=webp&thumbnail=246x0)`,
      }"
    ></div>
    <button class="suojin" @click.stop="$emit('toggle-show-play-page', false)">
      play-page
    </button>
    <section class="rotate">
      <img
       class="needle"
       :class="{ paused: !playing }"
       src="https://s3.music.126.net/mobile-new/img/needle-ab.png" 
       alt="">
    </section>
    <section class="record" :class="{ playing: playing }">
      <img
       class="thumb"
       :src="currentSong.song ? currentSong.picUrl : currentSong.al.picUrl" alt="">
       <img class="diso"
          src="https://s3.music.126.net/mobile-new/img/disc.png" alt="">
    </section>
  </section>
</template>

<script>
export default {
  props: {
    currentSong: Object,
    playing: Boolean,
    duration: Number,
    currentTime: Number,
  },
};
</script>

<style lang="less" scoped>
.play-page {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;

  .mask {
   
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    // 模糊
    filter: blur(50px) brightness(0.5);
    position:absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    //缩放
    transform: scale(1.2);
  }
  .suojin {
     z-index: 1;
  }
}
</style>