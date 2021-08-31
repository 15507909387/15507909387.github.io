<template>
 
    <section 
    class="playbar" 
    :class="{ playing: playing }"
    
    >
      <img
       @click="$emit('toggle-show-play-page', true)"
        class="song-picture"
        :src="`${
          currentSong.song ? currentSong.picUrl : currentSong.al.picUrl
        }?imageView=1&type=webp&thumbnail=80x0`"
        alt=""
      />
      <h3 @click="$emit('toggle-show-play-page', true)">
        {{ currentSong.name }}
        <span>
          -
          {{
            currentSong.song
              ? currentSong.song.artists[0].name
              : currentSong.ar[0].name
          }}</span
        >
      </h3>
      
      <div class="progress" @click="$emit('toggle-playing-state')">
        <canvas width="40px" height="40px" ref="canvas"></canvas>
        <img
          class="song-play"
          :class="[playing ? 'fa fa-pause' : 'fa fa-play']"
          src=""
          alt=""
        />
      </div>

      <button @click="$emit('toggle-show-play-list',true)">列表</button>
      
    </section>
    
</template>

<script>


export default {
  
  props: {
    currentSong: Object,
    playing: Boolean,
    duration: Number,
    currentTime: Number,
    currentPlayList: Array,
  },
 
  mounted: function () {
    //检查是否有canvas
    // console.log(this.$refs.canvas);
    // console.log(this.currentTime, this.duration);
  },

  computed: {
    percentage: function () {
      return this.currentTime / this.duration;
    },
  },
  watch: {
    percentage: function (n) {
      var context = this.$refs.canvas.getContext("2d");
      context.clearRect(0, 0, 40, 40);
      context.beginPath();
      context.arc(
        20,
        20,
        18,
        (Math.PI / 180) * (360 * n - 90),
        (Math.PI / 180) * (360 - 90)
      );
      context.strokeStyle = "lightgray";
      context.lineWidth = 1;
      context.stroke();

      context.beginPath();
      context.arc(
        20,
        20,
        18,
        (Math.PI / 180) * (0 - 90),
        (Math.PI / 180) * (360 * n - 90)
      );
      context.strokeStyle = "#d43c33";
      context.lineWidth = 1;

      context.stroke();
    },
  },
};
</script>

<style lang="less" scoped>

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.playbar {
  
    width: 100%;
    height: 50px;
    background: #fff;
    padding-bottom: 10px;
    position: fixed;
    bottom: 0;
    left: 0;
    display: flex;
    align-items: center;
    padding: 0 12px;

    // box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.15);
    img.song-picture {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      animation: rotate 8s linear infinite;
      animation-play-state: paused;
    }

    &.playing {
      img {
        animation-play-state: running;
      }
    }
    h3 {
      padding: 0 10px;
      flex: 1;
      font-size: 14px;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 1;
      overflow: hidden;
      text-overflow: ellipsis;
      span {
        color: rgb(151, 151, 151);
        font-size: 10px;
      }
    }

    .progress {
      width: 30px;
      height: 30px;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      canvas {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0px;
      }
      img {
        color: black;
        font-size: 12px;
      }
    }
  }
 
 

</style>