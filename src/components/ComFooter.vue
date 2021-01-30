<template>
  <footer :style="{ backgroundColor: curMenu.bgColor }">
    <div v-for="menu in menuList" :key="menu.path" @click="switchMenu(menu)">
      <router-link :to="menu.path">{{ menu.name }}</router-link>
    </div>
  </footer>
</template>

<script>
import { mapState,mapMutations } from "vuex";
export default {
    computed:{
        ...mapState(['curMenu'])
    },
  data() {
    return {
      curColor: "red",
      menuList: [
        { path: "/", name: "剧集", bgColor: "#ff0000" },
        { path: "/music", name: "音乐", bgColor: "#00ff00" },
        { path: "/book", name: "书籍", bgColor: "#0000ff" },
        { path: "/chat", name: "聊天", bgColor: "#f00889" },
      ],
    };
  },
  methods: { 
    ...mapMutations(['setCurMenu']),
    switchMenu(menu) {
      this.setCurMenu(menu)
    },
  },
};
</script>

<style lang="scss" scoped>
footer {
  position: fixed;
  display: flex;
  justify-content: space-around;
  bottom: 0;
  width: 100%;
  height: 1rem;
  line-height: 1rem;
  text-align: center;
  background: rgb(126, 23, 23);
}
</style>