<template>
  <div class="container flex-center">
    <div class="content flex-col-around">
      <div class="item flex-around">
        <span>用户名：</span>
        <input
          type="text"
          placeholder="请输入用户名"
          class="input-box"
          v-model="validateForm.name"
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
        <span>验证码:</span>
        <input
          type="text"
          class="vinput-box"
          placeholder="请填写验证码"
          v-model="validateForm.verifyCode"
          @focus="getVerify"
        />
        <img
          ref="image"
          src="http://localhost:8080/captcha"
          @click.prevent="refresh"
        />
      </div>
      <div class="item flex-around">
        <button class="btn" style="background" @click="login">
          登录
        </button>
        <button class="btn" @click="clear">重置</button>
      </div>
    </div>
    <div class="zheZhao flex-center" v-if="status">
      <div class="content flex-center">
        <h3>请选择您的身份：</h3>
        <div class="item flex-around">
          <!-- <button
            v-for="(role, index) in roles"
            :key="index"
            @click="gotoIndex(role.roleId)"
          >
            {{ role.roleName }}
          </button> -->
          <v-btn
            color="error"
            dark
            large
            v-for="(role, index) in roles"
            :key="index"
            @click="gotoIndex(role.roleId)"
          >
            {{ role.roleName }}</v-btn
          >
        </div>
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
        name: "",
        password: "",
        verifyCode: ""
      },
      menuList: [],
      roles: [],
      status: false
    };
  },
  components: {},
  created() {},
  mounted() {},
  methods: {
    getVerify() {
      console.log(this.validateForm.name);
      this.axios({
        method: "Get",
        url: this.GLOBAL.baseUrl + "/captcha",
        // url: "http://localhost:8080/captcha",
        //将请求数据转换为from-data格式
        params: {
          name: this.validateForm.name
        },
        //设置请求头Content-Type
        headers: { "Content-Type": "application/x-www-from-urlencoded" },
        responseType: "blob"
      }).then(res => {
        let img = this.$refs.image;
        let url = window.URL.createObjectURL(res.data);
        img.src = url;
      });
      console.log(this.validateForm.name);
    },
    login() {
      console.log(JSON.stringify(this.validateForm));
      this.axios({
        method: "Post",
        url: "http://localhost:8080/sysAdmin/login",
        headers: { "Content-Type": "application/json" },
        data: JSON.stringify(this.validateForm)
      }).then(res => {
        if (res.data.code === 1) {
          //存token
          localStorage.setItem("token", res.data.data.token);
          this.$store.commit("setToken", res.data.data.token);
          let admin = {
            id: res.data.data.admin.id,
            name: res.data.data.admin.name,
            roles: res.data.data.admin.roles,
            avatar: res.data.data.admin.avatar
          };
          //存admin
          localStorage.setItem("admin", JSON.stringify(admin));
          this.$store.commit("setAdmin", JSON.stringify(admin));
          this.roles = res.data.data.admin.roles;
          //角色数量超过1个
          if (this.roles.length > 1) {
            //显示遮罩层，遮罩层按钮具体点击事件 gotoDashboard(roleId)
            this.status = true;
          } else {
            //只有一个角色
            const roleId = res.data.data.admin.roles[0].roleId;
            alert(roleId);
            this.$router.push({
              path: "/",
              query: {
                roleId: roleId
              }
            });
          }
        } else {
          //登录失败
          alert(res.data.msg);
          //清空验证码文本框
          this.verifyCode = "";
        }
      });
    },
    refresh() {
      //点击验证码实现刷新的功能
      this.axios
        .get(this.GLOBAL.baseUrl + "/captcha?name=" + this.validateForm.name, {
          responseType: "blob"
        })
        .then(res => {
          let img = this.$ref.image;
          let url = window.URL.createObjectURL(res.data);
          img.src = url;
        });
    },
    gotoIndex(roleId) {
      this.$router.push({
        path: "/",
        query: {
          roleId: roleId
        }
      });
    },
    // submit() {
    //   //模拟后端接口数据
    //   let user = {
    //     userId: "200004021629",
    //     username: "ychsang123",
    //     userRole: "admin",
    //     avatar:
    //       "https://avatars3.githubusercontent.com/u/55419799?s=400&u=06400cc24a3dd8b97880a631daad51e37a6792d8&v=4"
    //   };
    //   this.menuList = [
    //     {
    //       title: "Dashboard",
    //       icon: "mdi-view-dashboard",
    //       url: "/dashboard",
    //       subMenus: []
    //     },
    //     {
    //       title: "音乐管理",
    //       icon: "mdi-music",
    //       url: "",
    //       subMenus: [
    //         {
    //           title: "歌单管理",
    //           icon: "mdi-domain",
    //           url: "/music-list",
    //           permissions: []
    //         },
    //         {
    //           title: "歌曲管理",
    //           icon: "mdi-text",
    //           url: "/music",
    //           permissions: ["music:add", "music:edit", "music:delete"]
    //         }
    //       ]
    //     },
    //     { title: "About", url: "/about", subMenus: [] }
    //   ];
    //   localStorage.setItem(
    //     "token",
    //     "EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M"
    //   );
    //   localStorage.setItem("user", JSON.stringify(user));
    //   localStorage.setItem("menuList", JSON.stringify(this.menuList));
    //   this.$store.commit(
    //     "setToken",
    //     "EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M"
    //   );
    //   this.$store.commit("setUser", user);
    //   this.$store.commit("setMenuList", this.menuList);
    //   this.$router.push("/");
    // },
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
.vinput-box {
  width: 35%;
  height: 100%;
  border: none;
  border-bottom: 1px solid #757575;
}
.container {
  background-image: url("https://images.unsplash.com/photo-1570103465173-0f1fe55b55ff?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60");
  background-size: 100%;
  height: 100vh;
}
.grayFont {
  color: #757575;
  font-size: 16px;
}
.zheZhao {
  width: 100%;
  height: 100vh;
  background: rgba(33, 33, 33 0.5);
  position: relative;
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
