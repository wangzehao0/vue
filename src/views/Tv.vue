<template>
  <div>
    <ul>
      <li
        v-for="tv in tvList"
        :key="tv.id"
        class="tv-item"
        @click="getDetail(tv.id)"
      >
        <div class="img-box">
          <img :src="tv.cover.url" alt="" />
        </div>
        <div class="content">
          <h3>
            {{ tv.title }}
          </h3>
          <p>
            {{ tv.info }}
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Tv",
  created() {
    this.getData();
  },
  data() {
    return {
      tvList: [],
      start: 0,
      isFinish: true,
    };
  },
  mounted() {
    this.lazyLoad();
  },
  methods: {
    //ajax请求接口
    //跨域问题：当你网页所在地址和接口的地址（协议，域名，端口号），任何一个不同都会引发跨域问题（浏览器同源策略）
    //解决：jsonp,后端配置(cors),第三方服务器代理
    getData() {
      if (this.isFinish) {
        this.isFinish = false;
        this.axios
          .get(
            "https://bird.ioliu.cn/v2?url=https://m.douban.com/rexxar/api/v2/subject_collection/tv_domestic/items",
            {
              params: {
                start: this.start,
                count: 10,
              },
            }
          )
          .then((res) => {
            this.tvList = this.tvList.concat(res.data.subject_collection_items);
          });
      }
    },
    lazyLoad() {
      //页面的高度
      let htmlDom = document.documentElement;
      let fullHeight = 0;
      //设备的高度
      let deviceHeight = htmlDom.clientHeight;
      //滚动出去的距离
      let scrollHeight = 0;
      window.onscroll = () => {
        fullHeight = htmlDom.offsetHeight;
        scrollHeight = htmlDom.scrollTop;
        console.log(fullHeight, deviceHeight, scrollHeight);
        //滚动到底部
        if ((fullHeight = deviceHeight + scrollHeight)) {
          if (this.start < 4) {
            //请求下一页数据
            this.start++;
            this.getData();
          }
        }
      };
    },
    //跳转电视详情页
    getDetail(id) {
      this.$router.push("/tvDetail/" + id);
    },
  },
};
</script>

<style lang="scss" scoped>
.tv-item {
  display: flex;
  padding: 10px 0;
  border-bottom: 1px solid #ccc;
  .img-box {
    flex: 3;
    img {
      width: 100%;
      height: 100%;
    }
  }
  .content {
    flex: 5;
    padding: 0.8rem;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    h3 {
      font-weight: 800;
    }
  }
}
</style>