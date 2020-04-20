<template>
  <div class="container flex-center">
    <div class="content flex-col-around">
      <div class="item flex-around">
        <span>用户名：</span>
        <input
          type="text"
          placeholder="请输入用户名"
          class="input-box"
          v-model="validateForm.username"
        />
      </div>
      <div class="item flex-around">
        <span>密码：</span>
        <input
          type="password"
          class="input-box"
          placeholder="请输入密码"
          v-model="validateForm.password"
        />
      </div>
      <div class="item flex-around">
        <button class="btn" style="background" @click="submit">
          <router-link to="/dashboard">登录</router-link>
        </button>
        <button class="btn" @click="clear">重置</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      validateForm: {
        username: "",
        password: ""
      },
      menuList: []
    };
  },
  components: {},
  created() {},
  mounted() {},
  methods: {
    submit() {
      //模拟后端接口数据
      let user = {
        userId: "200004021629",
        username: "ychsang123",
        userRole: "admin",
        avatar:
          "https://avatars3.githubusercontent.com/u/55419799?s=400&u=06400cc24a3dd8b97880a631daad51e37a6792d8&v=4"
      };
      this.menuList = [
        {
          title: "Dashboard",
          icon: "mdi-view-dashboard",
          url: "/dashboard",
          subMenus: []
        },
        {
          title: "音乐管理",
          icon: "mdi-music",
          url: "",
          subMenus: [
            {
              title: "歌单管理",
              icon: "mdi-domain",
              url: "/music-list",
              permissions: []
            },
            {
              title: "歌曲管理",
              icon: "mdi-text",
              url: "/music",
              permissions: ["music:add", "music:edit", "music:delete"]
            }
          ]
        },
        { title: "About", url: "/about", subMenus: [] }
      ];
      localStorage.setItem(
        "token",
        "EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M"
      );
      localStorage.setItem("user", JSON.stringify(user));
      localStorage.setItem("menuList", JSON.stringify(this.menuList));
      this.$store.commit(
        "setToken",
        "EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M"
      );
      this.$store.commit("setUser", user);
      this.$store.commit("setMenuList", this.menuList);
      this.$router.push("/");
    },
    clear() {
      this.validateForm = {
        username: "",
        password: ""
      };
    }
  },
  computed: {}
};
</script>

<style scoped>
* {
  margin: 0 auto;
}
.content {
  width: 40%;
  height: 300px;
  background-color: wheat;
  border-radius: 10px;
  position: absolute;
  z-index: 10px;
}
.item {
  width: 80%;
  height: 60px;
}
.input-box {
  width: 70%;
  height: 100%;
  border: none;
  border-bottom: 1px solid #757575;
}
.container {
  background-image: url("https://images.unsplash.com/photo-1570103465173-0f1fe55b55ff?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60");
  background-size: 100%;
  height:100vh;

}
.grayFont {
  color: #757575;
  font-size: 16px;
}
.btn {
  width: 30%;
  height: 100%;
  border-radius: 5px;
  color: white;
  font-size: 14px;
  font-weight: 600;
}
.flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
.flex-col-around {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.flex-around {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>
