<template>
  <div class="search">
                                <!-- keyup 按键修饰符 enter 回车 -->
    <input 
      type="text" 
      placeholder="搜索" 
      v-model.trim="value" 
      @keyup.enter="value && (searching = true)"/>
    <section class="hots" v-if="!value && !searching" >
      <h5>热门搜索</h5>
      <ul class="">
        <li v-for="hot in hots" :key="hot.first">{{ hot.first }}</li>
      </ul>
    </section>
    <section class="suggest" v-if="value && !searching">
      <h5>搜索建议</h5>
      <ul v-if="searching">
        <li 
          v-for="suggestss in suggests" 
          :key="suggestss.feature"
          @click="
            value = suggestss.keyword;
            searching = true;
            "
          >
          {{ suggestss.keyword }}
        </li>
      </ul>
    </section>
    <section class="suggest" v-if="searching">
      <h5>搜索结果</h5>
      <ul>
        <li>
          
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      hots: [],
      value: "",
      suggests: [],
      searching: false,
    };
  },
  created: function () {
    this.axios.get("http://apis.netstart.cn/music/search/hot").then((res) => {
      console.log(res);
      this.hots = res.data.result.hots;
    });
  },
  watch: {
    value: function (n) {
      if (n) {
        //ajax 参数对象的形式
        this.axios
          .get("http://apis.netstart.cn/music/search/suggest", {
            params: {
              keywords: n,
              type: "mobile",
            },
          })
          .then((res) => {
            this.suggests = res.data.result.allMatch;
          });
      }
    },
  },
};
</script>

<style lang="less" scoped>
.search {
  width: 100vw;
  padding: 15px;
  input {
    width: 100%;
    height: 30px;
    border-radius: 30px;
  }

  .hots {
    ul {
      li {
        float: left;
        border: 0.2px solid rgb(219, 216, 216);
        border-radius: 20px;
        height: 1.5em;
        padding: 0 10px;
        margin: 8px;
        line-height: 1.5em;
      }
    }

    .suggest {
         clear: both;
     
    }
  }
}
</style>