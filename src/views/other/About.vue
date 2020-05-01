<template>
  <div class="container">
    <div class="left">
      <img :src="admin.avatar" class="avatar" />
    </div>
    <div class="right">
      <div class="btngroup" style="display:flex">
        <v-btn large color="#ff5252" class="btnColor" @click="change"
          >修改</v-btn
        >
        <v-btn large color="#4caf50" class="btnColor" @click="updateUser"
          >保存</v-btn
        >
      </div>
      <div class="item">
        <h4>用户名：</h4>
        <span v-if="status">{{ admin.name }}</span>
        <input type="text" v-model="admin.name" class="textBox" v-else />
      </div>
      <div class="item">
        <h4>手机号：</h4>
        <span v-if="status">{{ admin.phone }}</span>
        <input type="text" v-model="admin.phone" class="textBox" v-else />
      </div>
      <div class="item">
        <h4>性别：</h4>

        <span v-if="status">{{ admin.gender }}</span>
        <input type="text" v-model="admin.gender" class="textBox" v-else />
      </div>
      <div class="item">
        <h4>居住地：</h4>
        <span v-if="status">{{ admin.address }}</span>
        <input type="text" v-model="admin.address" class="textBox" v-else />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "About",
  data() {
    return {
      admin: JSON.parse(localStorage.getItem("admin")),
      status: true,
      phoneMessage: "",
      genderMessage: ""
    };
  },
  components: {},
  created() {
    // this.$store.commit("setAdmin", JSON.parse(localStorage.getItem("admin")));
    console.log(this.admin);
    console.log(this.admin.id);
  },
  mounted() {},
  methods: {
    change() {
      this.status = false;
    },
    updateUser() {
      this.status = true;
      this.axios({
        method: "post",
        url: this.GLOBAL.baseUrl + "/api/update",
        data: {
          id: this.admin.id,
          name: this.admin.name,
          phone: this.admin.phone,
          gender: this.admin.gender,
          address: this.admin.address
        }
      }).then(console.log("信息修改成功"), (this.status = true));
    }
  },
  computed: {}
};
</script>

<style scoped lang="scss">
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.item {
  display: flex;
  border-bottom: 1px solid #e0e0e0;
  height: 100px;
  align-items: center;
}
.left {
  width: 20%;
  height: 100vh;
}
.right {
  width: 60%;
  height: 100vh;
}
.avatar {
  width: 80%;
  height: 160px;
  border-radius: 10px;
}
.btngroup {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.btnColor {
  color: white;
  font-weight: 600;
}
.textBox {
  width: 200px;
  height: 50px;
  border: 2px solid #bdbdbd;
  border-radius: 5px;
  color: #616161;
  font-size: 18px;
}
</style>
