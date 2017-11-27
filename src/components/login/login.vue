<template>
  <div>
    <div class="login" v-if="!isLogin">
      <p>
        <input type="text" name="" v-model="loginName" >
      </p>
      <p>
        <input type="password" name="" v-model="passWord">
      </p>
      <p>
        <button @click="login()">login</button>
      </p>
    </div>
    <div class="main" v-else>
      <p>
        <a href="./report.html" target="_blank">report</a>
      </p>
      <p>
        <a href="./exam.html" target="_blank">exam</a>
      </p>
      <p>
        <button @click="loginOut()">loginOut</button>
      </p>
    </div>
  </div>
</template>

<script>
import "@/assets/normalize.css";
import storage from "@/common/storage";
import { userLogin, userLogout } from "@/common/httpServers";
export default {
  name: "login",
  data() {
    return {
      loginName: "",
      passWord: "",
      isLogin: false
    };
  },
  created() {
    this.isLogin = storage.getStorage("isLogin") === "true";
  },
  methods: {
    login() {
      if (this.loginName && this.passWord) {
        userLogin(this.loginName, this.passWord).then(res => {
          if (res.data.code == 200) {
            this.isLogin = true;
            storage.setStorage("isLogin", "true");
            storage.setStorage("userInfo", {
              loginName: this.loginName,
              passWord: this.passWord
            });
          } else {
            alert(res.data.message);
          }
        });
      } else {
        console.log("error");
      }
    },
    loginOut() {
      userLogout().then(res => {
        if (res.data.code == 200) {
          storage.removeStorage("isLogin");
          storage.removeStorage("userInfo");
          this.resetFrom();
        } else {
          alert(res.data.message);
        }
      });
    },
    resetFrom() {
      this.loginName = "";
      this.passWord = "";
      this.isLogin = false;
    }
  }
};
</script>
<style  scoped>
p {
  text-align: center;
}
</style>
