<template>
  <div style="padding-top:10px;">
    <span v-for="(item, index) in menus" :key="index" class="gutter">
      <mu-button :color="item.icon">{{ item.title }}</mu-button>
    </span>
    <!-- <v-card>
      <v-card-title>
        Nutrition
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
      ></v-data-table>
    </v-card> -->
  </div>
</template>

<script>
export default {
  name: "Music",
  data() {
    return {
      menus: [],
      types: [],
      child: [],
      search: ""
    };
  },
  components: {},
  created() {
    let index = this.$route.query.index;
    let index1 = this.$route.query.index1;
    console.log(index, index1);
    this.menus = JSON.parse(localStorage.getItem("menuList"))[index].subMenus[
      index1
    ].subMenus;
    console.log(this.menus);
    this.getAll();
    console.log(this.types);
    console.log(this.child);
  },
  mounted() {},
  methods: {
    getAll() {
      this.axios({
        method: "get",
        url:
          this.GLOBAL.baseUrl +
          "/songList/all?roleId=" +
          localStorage.getItem("roleId"),
        headers: { Authorization: localStorage.getItem("token") }
      }).then(res => {
        this.types = res.data.data;
        this.child = this.types.child;
      });
    }
  },
  computed: {}
};
</script>

<style scoped lang="scss"></style>
