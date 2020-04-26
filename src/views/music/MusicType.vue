<template>
  <div>
    <h3>歌曲类型管理</h3>
    <span v-for="(item, index) in menus" :key="index" class="gutter">
      <mu-button :color="item.icon">{{ item.title }}</mu-button>
    </span>
    <div class="container">
      <div class="bar">
        <button
          depressed
          small
          class="btn"
          v-for="(item, index) in types"
          :key="index"
          @click="getMusic(item.type)"
        >
          {{ item.type }}
        </button>
      </div>
      <div class="card">
        <div class="item" v-for="(music, index) in musicItem" :key="index">
          <div class="top">
            <img :src="music.thumbnail" />
            <div class="songTitle">
              <span class="titleFont">{{ music.songListName }}</span>
            </div>
          </div>
          <div class="bottom">
            <span class="grayFont">{{ music.type }}</span>
            <span class="grayFont">{{ music.likeCount }}</span>
            <span class="grayFont">{{ music.playCount }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MusicType",
  data() {
    return {
      menus: [],
      types: [],
      musicItem: []
    };
  },
  created() {
    let index = this.$route.query.index;
    let index1 = this.$route.query.index1;
    this.menus = JSON.parse(localStorage.getItem("menuList"))[index].subMenus[
      index1
    ].subMenus;
    // this.getAll();
    console.log(localStorage.getItem("roleId"));
    this.getAll();
  },
  methods: {
    getAll() {
      this.axios({
        method: "get",
        url:
          this.GLOBAL.baseUrl +
          "/songList/all?roleId=" +
          localStorage.getItem("roleId")
      }).then(res => {
        this.types = res.data.data;
      });
    },
    getMusic(field) {
      this.axios({
        method: "get",
        url:
          this.GLOBAL.baseUrl +
          "/songList/search?roleId=" +
          localStorage.getItem("roleId"),
        params: {
          field: field
        }
      }).then(res => {
        this.musicItem = res.data.data;
        console.log(this.musicItem);
      });
    }
  }
};
</script>

<style scoped lang="scss">
.btn {
  background-color: #0097a7;
  width: 120px;
  height: 40px;
  font-size: 14px;
  color: white;
  border: none;
}
.container {
  width: 90%;
  height: 100vh;
  display: flex;
  display: flex;
  flex-direction: column;
}
.top {
  width: 100%;
  height: 80%;
  position: relative;
}
.songTitle {
  height: 20%;
  position: absolute;
  width: 100%;
  z-index: 3;
  bottom: 20px;
}
.top img {
  width: 100%;
  height: 100%;
  border-radius: 10px;
}
.bottom {
  width: 100%;
  height: 20%;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.card {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
  margin-top: 20px;
}
.item {
  width: 28%;
  height: 220px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.bar {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.grayFont {
  font-size: 16px;
  color: #607d8b;
}
.titleFont {
  font-size: 18px;
  font-weight: 600;
  color: white;
}
</style>
