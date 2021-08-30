<template>
  <div class="play-page">
    <!-- 点击.bar显示 -->
   
      <section class="play-page">
        <div>12234</div>
        <button @click.stop="$emit('toggle-show-play-page', false)">play-page</button>
      </section>
  </div>
</template>

<script>
export default {
  posps: {
    
  }
};
</script>

<style lang="less" scoped>
 .play-page {
    width: 100vh;
    height: 100vh;
    position: fixed;
    left: 0;
    top: 0;
    background: red;
  }
</style>