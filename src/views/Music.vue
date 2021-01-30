<template>
  <div>
    <!--music：当前播放的音乐。 list：播放列表 ：showlrc：是否显示歌词-->
    <aplayer :music="audio[0]" :list="audio" :showlrc="false"></aplayer>
  </div>
</template>

<script>
import aplayer from "vue-aplayer";
export default {
  created() {
    this.getData();
  },
  components: { aplayer },
  data() {
    return {
      // 音频列表
      audio: [],
      playList: [],
    };
  },
  methods: {
    getData() {
      this.axios
        .get("https://bird.ioliu.cn/netease/playlist?id=19723756")
        .then((res) => {
          console.log(res);
          this.playList = res.data.playlist.tracks;
          this.audio = this.playList.map((item, ) => {
            return {
              title: item.name,
              artist: item.ar[0].name,
              url: "",
              pic: item.al.picUrl,
              lrc: "",
              id:''
            };
          });
        });
    },
  },
};
</script>

<style lang="scss" scoped>
</style>