<template>
  <div class="search" @scroll="scroll">
    <!-- keyup 按键修饰符 enter 回车 -->
    <!-- 当元素获得焦点时（当通过鼠标点击选中元素或通过 tab 键定位到元素时），发生 focus 事件。 -->
    <!-- 当元素失去焦点时发生 blur 事件。 -->
    <input
      type="text"
      placeholder="搜索"
      v-model.trim="value"
      @keyup.enter="value && (searching = true)"
      @focus="inputing = true"
      @blur="inputing = false"
    />
    <section class="hots" v-if="!value && !searching">
      <h5>热门搜索</h5>
      <ul class="">
        <li v-for="hot in hots" :key="hot.first" @click="value = hot.first">
          {{ hot.first }}
        </li>
      </ul>
    </section>
    <section class="record">
      <ol v-if="!value && !searching">
        <li
          v-for="his in history"
          :key="his"
          @click="
            searching = true;
            value = his;
          "
        >
          {{ his }}
        </li>
      </ol>
    </section>
    <section class="suggest" v-if="value && !searching">
      <h5>搜索建议</h5>
      <ul>
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
        <li
          v-for="(item, index) in searchResults"
          :key="index"
          @click="
            value = item.keyword;
            searching = true;
          "
        >
          {{ item.name }}
        </li>
      </ul>
      <p v-if="!hasMore">没有更多了</p>
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
      searchResults: [],
      searching: false,
      inputing: false,
      page: 0,
      hasMore: false,
      history: JSON.parse(window.localStorage.getItem("history")) || [],
    };
  },

  methods: {
    //判断是否触底
    scroll: function (event) {
      // console.log(event);
      // console.log(event.target.offsetHeight);
      // console.log(event.target.scrollTop);
      // console.log(event.target.scrollHeight);

      if (
        event.target.offsetHeight + event.target.scrollTop ===
        event.target.scrollHeight
      ) {
        console.log("底部");
        this.getSearch();
      }
    },

    //搜索结果
    getSearch: function () {
      this.axios
        .get("http://apis.netstart.cn/music/search", {
          params: {
            keywords: this.value,
            limit: 30,
            offset: this.page * 30,
          },
        })
        .then((res) => {
          this.searchResults.push(...res.data.result.songs);
          // 触底加页
          this.page++;
          this.hasMore = res.data.result.hasMore;
          //  console.log(res.data);

          //历史记录
          this.history = [...new Set([...this.history, this.value])];
          window.localStorage.setItem("history", JSON.stringify(this.history));
        });
    },
  },

  created: function () {
    this.axios.get("http://apis.netstart.cn/music/search/hot").then((res) => {
      // console.log(res);
      this.hots = res.data.result.hots;
    });
  },
  watch: {
    value: function (n) {
      if (this.inputing) {
        this.searching = false;
      }
      if (n && !this.searching) {
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
      } else {
        this.suggests = [];
      }
    },

    searching: function (n) {
      if (n && this.value) {
        //ajax 参数对象的形式
        this.getSearch();
      } else {
        this.searchResults = [];
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
  }
  .record {
      clear: both;
      margin-left: 15px;
    }

    .suggest {
      clear: both;
    }
}
</style>