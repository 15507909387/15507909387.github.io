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
        alt=""
      />

      <section class="record" :class="{ playing: playing }">
        <img
          class="thumb"
          :src="currentSong.song ? currentSong.picUrl : currentSong.al.picUrl"
          alt=""
        />
        <img
          class="diso"
          src="https://s3.music.126.net/mobile-new/img/disc.png"
          alt=""
        />
      </section>
      <section class="controls">
        <span @click="$emit('page-top')">上一首</span>
        <span @click="$emit('toggle-playing-state')">播放</span>
        <span @click="$emit('page-bottom')">下一首</span>
        <span @click="$emit('toggle-show-play-list',true)">列表</span>
      </section>
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
//复用样式
.pos-ab() {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
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
    filter: blur(30px) brightness(0.5);
    z-index: -1;
    //缩放
    transform: scale(1.2);
    .pos-ab();
  }

  .rotate {

    position: relative;
    padding-top: 25vw;
    top:40px;
    img.needle {
      height: 40vw;
      position: absolute;
      top: 0;
      left: 48%;
      z-index: 1;
      margin-left: -15px;
      transform-origin: 15px 15px;
      transform: rotate(0deg);
      transition: all 0.3s;

      &.paused {
        transform: rotate(-20deg);
      }
    }
  }

  .record {
    position: relative;
    width: 60vw;
    height: 60vw;
    left: 20vw;
    img {
      .pos-ab();
      border-radius: 50%;
    }
    img.thumb {
      transform: scale(0.6);
    }
    animation: rotate 8s linear infinite;
    animation-play-state: paused;
    &.playing {
      animation-play-state: running;
    }
  }

  .controls {
    width: 100%;
    // background: red;
    padding: 10px;
    display: flex;
    justify-content: space-around;
    color: white;
    top: 93vh;
    position: fixed;
    height: 7vh;
  }
}
</style>