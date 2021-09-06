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
    <section class="rotate" @click="showLyric = true" v-if="!showLyric">
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
    </section>
    <section class="lyric" v-else @click="showLyric = false" ref="lyric">
      <ul class="content" ref="lyricContent" v-if="parsedLyric.length">
        <li v-for="(l, i) in parsedLyric" :key="i">
          <span
            :class="{
              active:  currentLyricIndex === i,
              
            }"
            >{{ l.text }}</span
          >
        </li>
      </ul>
    </section>
    <!-- duration 总时长  setp 步进-->
    <section class="progress">
      <input
        type="range"
        :max="duration"
        step="0.5"
        v-model="value"
        @change="progressChange"
        @input="progressInput"
      />
      <span
        class="bar"
        :style="{ width: (value / duration) * 100 + '%' }"
      ></span>
      <span
        class="circle"
        :style="{ left: (value / duration) * 100 + '%' }"
      ></span>
    </section>
    <section class="controls">
      <span @click="$emit('page-top')">上一首</span>
      <span @click="$emit('toggle-playing-state')">播放</span>
      <span @click="$emit('page-bottom')">下一首</span>
      <span @click="$emit('toggle-show-play-list', true)">列表</span>
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

  data: function () {
    return {
      value: this.currentTime,
      inputing: false,
      showLyric: false,
      lyric: null,
      lisH: [],
    };
  },

  watch: {
    currentTime: function (n) {
      if (!this.inputing) {
        this.value = n;
      }
    },

    "currentSong.id": function (id) {
      // console.log(id);
      this.getLyric(id);
    },

    currentLyricIndex: function () {
      // var lis = this.$refs.lyricContent
      //   ? this.$refs.lyricContent.querySelectorAll("li")
      //   : null;
      // // console.log(index, lis);
      // if (lis != null) {
      //   this.lisH = [...lis].map((item) => item.offsetHeight);
      // }
      // console.log(this.lisH);
      console.log("aaa");
    },

    parsedLyric: function () {
      // console.log("parsedLyric改变", this.$refs.lyricContent);
      // this.$nextTick(() => {
      //   console.log("nextTick", this.$refs);
      //   var lis = this.$refs.lyricContent.querySelectorAll("li");
      //   this.lisH = [...lis].map((item) => item.offsetHeight);
      //   console.log(this.lisH);
      // });
      //  当前歌词前面所有歌词高度
      // var h = this.lisH.slice(0, index).reduce(function(total, item) {
      //   return total + item;
      // }, 0);
      // console.log(h);
      // var lis = this.$refs.lyricContent
      //   ? this.$refs.lyricContent.querySelectorAll("li")
      //   : null;
      // console.log(lis);
      // if (lis != null) {
      //   this.lisH = [...lis].map((item) => item.offsetHeight);
      // }
      // console.log(this.lisH);
      console.log("111");
    },
  },
  created: function () {
    this.getLyric(this.currentSong.id);
    // console.log(this.$refs.lyricContent);
  },

  methods: {
    progressChange: function (event) {
      this.inputing = false;
      this.$emit("current-time-change", event.target.value);
    },
    progressInput: function () {
      this.inputing = true;
    },

    getLyric: function (id) {
      this.axios
        .get("/lyric", {
          params: { id },
        })
        .then((res) => (this.lyric = res.data.lrc.lyric));
    },
  },

  computed: {
    parsedLyric: function () {
      if (this.lyric) {
        return this.lyric
          .split("\n")
          .filter((s) => s)
          .map((s) => {
            var text = s.replace(/^\[\d{2}:\d{2}\.\d{2,3}\]/i, "");
            var timeStr = s.replace(text, "").replace(/(^\[|\]$)/g, "");
            var timeArr = timeStr.split(":").map((item) => Number(item));
            var time = timeArr[0] * 60 + timeArr[1];
            return { text, time };
          });
      } else {
        return [];
      }
    },
    currentLyricIndex: function () {
      var i = this.parsedLyric.findIndex(
        (item) => item.time > this.currentTime
      );
      var currentLyricIndex = i !== -1 ? i - 1 : this.parsedLyric.length - 1;
      return currentLyricIndex;
    },
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
    height: 75vh;
    position: relative;
    padding-top: 25vw;
    top: 40px;
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

  .lyric {
    height: 70vh;
    background: rgb(116, 115, 115);
    text-align: center;
    overflow: hidden;
    position: relative;
    margin-top: 5vh;

    ul {
      transition: all 0.3s;
      background: blue;
      // position:absolute;
      bottom: 10vh;

      li {
        line-height: 1.8;

        span {
          &.active {
            color: red;
          }
        }
      }
    }
  }

  .progress {
    width: 98vw;
    height: 4px;
    margin: 1vw auto;
    margin-top: 5vh;
    background: rgb(180, 180, 180);
    border-radius: 10px;
    position: relative;
    input {
      width: 100vw;
      left: -4px;
      background: red;
      position: absolute;
      top: -5px;
      opacity: 0;
    }
    .bar {
      display: block;
      background: #fff;
      border-radius: inherit;
      height: 4px;
      position: absolute;
    }
    .circle {
      display: block;
      width: 8px;
      height: 8px;
      background: rgb(255, 255, 255);
      border-radius: 50%;
      margin-left: -5px;
      position: absolute;
      top: -2px;
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