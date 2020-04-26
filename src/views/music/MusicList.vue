<template>
  <v-container class="pa-1" style="height = 1000px">
    <span v-for="(menu, index) in menus" :key="index" class="gutter">
      <mu-button :color="menu.icon">{{ menu.title }}</mu-button>
    </span>
    <v-row>
      <v-card
        class="mx-auto"
        max-width="250"
        v-for="(item, index1) in items"
        :key="index1"
      >
        <v-img
          class="white--text align-end"
          height="150px"
          :src="item.thumbnail"
        >
        </v-img>
        <v-card-title>{{ item.songListName }}</v-card-title>
      </v-card>
    </v-row>
    <template>
      <div class="text-center">
        <v-pagination v-model="page" :length="pages"></v-pagination>
      </div>
    </template>
  </v-container>
</template>

<script>
export default {
  name: "MusicList",
  data() {
    return {
      items: [],
      menus: [],
      pages: 0,
      page: 0
    };
  },
  created() {
    alert("开始");
    let index = this.$route.query.index;
    let index1 = this.$route.query.index1;
    this.menus = JSON.parse(localStorage.getItem("menuList"))[index].subMenus[
      index1
    ].subMenus;
    // let role = this.$store.state.admin.roles[0].roleId
    // console.log(this.role)
    this.getPage();
  },
  watch: {
    page: function() {
      this.getPage();
    }
  },
  methods: {
    getPage() {
      this.axios({
        method: "post",
        url:
          this.GLOBAL.baseUrl +
          "/songList/page?roleId=" +
          localStorage.getItem("roleId"),
        data: {
          currentPage: this.page,
          pageSize: 12
        },
        headers: { Authorization: localStorage.getItem("token") }
      })
        .then(res => {
          this.pages = res.data.data.pages;
          this.items = res.data.data.data;
          console.log(this.pages);
          console.log(this.items);
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

<style scoped lang="scss"></style>
