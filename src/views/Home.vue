<template>
  <div class="home">
    <div class="aaa">
      <div class="top">
      <div class="top_one">
        <van-icon name="chat-o" color="#1989fa" size="30" />
      </div>
      <div class="top_two">今日头条</div>
      <div class="top_three">
        <van-icon name="search" size="30" @click="go"/>
      </div>
    </div>
    <div class="scroll">
      <ul class="ul">
        <li
          v-for="(item,index) in tab"
          :key="index"
          @click="huan(item,index)"
          :class="{'active':index==Index}"
        >{{item}}</li>
      </ul>
    </div>
    </div>
    <div class="z"></div>
    <div class="content" v-for="(item,index) in show" :key="index">
      <div class="left"><img :src="item.imageUrls" class="img"></div>
      <div class="right"><div class="a">{{item.title}}</div><div class="bottom">来源：{{item.posterScreenName}}&nbsp;&nbsp;评论数：{{item.commentCount}}&nbsp;&nbsp;发布时间：{{item.publishDate}}</div></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {
      tab: [
        "推荐",
        "视频",
        "热点",
        "社会",
        "娱乐",
        "军事",
        "科技",
        "汽车",
        "房产",
        "家居",
        "体育",
        "财经"
      ],
      Index: 0,
      text: "",
      show: []
    };
  },
  methods: {
    huan(el, i) {
      this.Index = i;
      this.text = el;
    },
    go(){
      this.$router.push({
        path:'/search'
      })
    }
  },
  watch: {
    text(nval, oval) {
      console.log(nval);
      this.$axios
        .get("http://api.kudesoft.cn/news/list", { params: { kw: nval } })
        .then(res => {
          console.log(res.data.result.data);
          this.show = res.data.result.data;
        })
        .catch(err => {});
    }
  },
  created() {
    console.log(this.tab[0]);
    this.$axios
      .get("http://api.kudesoft.cn/news/list", { params: { kw: this.tab[0] } })
      .then(res => {
        console.log(res.data.result.data);
        this.show = res.data.result.data;
      })
      .catch(err => {});
  }
};
</script>
<style scoped>
.content{
  width: 100%;
  height: 100px;
  display: flex;
}
.aaa{
  position: fixed;
  top: 0;
  left: 0;
  padding-bottom: 100px;
}
.z{
  height: 15vh;
}
.bottom{
  margin-top: 20px;
  font-size: 13px;
}
.img{
  width: 100px;
  height: 100px;
}
.top {
  width: 100%;
  height: 10vh;
  background: rgb(221, 89, 89);
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.top_one,
.top_three {
  width: 10%;
  height: 100%;
}
.a{
  display: flex;
  justify-content: flex-start;
  text-align: left;
}
.top > div {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.top_two {
  width: 40%;
  height: 100%;
}
.scroll {
  width: 100vw;
  height: 50px;
  overflow: scroll;
}
.ul {
  width: 200vw;
  height: 5vh;
  background: white;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.ul li {
  width: 100%;
  height: 30px;
}
.active {
  border-bottom: 1px solid red;
}
</style>
