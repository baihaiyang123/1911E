<template>
  <div>
    <div>
      <van-search v-model="value" placeholder="请输入搜索关键词" />
    </div>
    <div class="h">
      <p style="width:100%;text-align:left">历史记录</p>
      <div class="hh">
        <van-tag type="primary" v-for="(item,index) in h" :key="index">{{item}}</van-tag>
      </div>
    </div>
    <div>
      <van-cell-group>
        <van-cell :title="item.title" v-for="(item,index) in show" :key="index" @click="a(item)" />
      </van-cell-group>
    </div>
    <div class="content" v-for="(item,index) in content" :key="index">
      <div class="left">
        <img :src="item.imageUrls" class="img" />
      </div>
      <div class="right">
        <div class="a">{{item.title}}</div>
        <div
          class="bottom"
        >来源：{{item.posterScreenName}}&nbsp;&nbsp;评论数：{{item.commentCount}}&nbsp;&nbsp;发布时间：{{item.publishDate}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: "",
      show: [],
      content: [],
      h: []
    };
  },
  methods: {
    a(item) {
      this.show = [];
      this.content.push(item);
      // console.log("acsacan女哦啊了困难大V咯看的 ")
      this.h.push(this.value)
      localStorage.aa = JSON.stringify(this.h);
      console.log(this.h);
    }
  },
  watch: {
    value(nval, oval) {
      if (this.value == "") {
        this.show = [];
        this.content = [];
      }
      console.log(nval);
      this.$axios
        .get("http://api.kudesoft.cn/news/list", {
          params: { kw: nval }
        })
        .then(res => {
          console.log(res.data.result.data);
          let all = res.data.result.data;
          all.forEach(el => {
            // console.log(el.title);
            if (el.title.includes(nval)) {
              this.show.push(el);
            }
          });
        })
        .catch(err => {});
    }
  },
  created() {
    let a = localStorage.aa;
    if (a) {
      this.h = JSON.parse(a);
    }
  }
};
</script>

<style scoped>
.h {
  width: 100%;
}
.hh {
  display: flex;
}
.img {
  width: 100px;
  height: 100px;
}
.bottom {
  margin-top: 20px;
  font-size: 13px;
}
.content {
  width: 100%;
  height: 100px;
  display: flex;
}
</style>