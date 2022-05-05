<template>
  <div id="login" class="login">
    <img class="bgbox" id="bgbox" alt="" src="../../src/assets/2.gif">
    <div class="wrap" style="width:400px;height:600px;margin:100px auto;text-align:center">
      <h1>注册用户</h1>
      <el-form :model="form" ref="form" class="form">
        <el-form-item prop="username">
          <el-input placeholder="用户名" type="username" v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item prop="email">
          <el-input placeholder="邮箱" type="email" v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item id="password" prop="password">
          <el-input
              placeholder="密码"
              show-password
              type="password"
              v-model="form.password"
              autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item id="password" prop="password">
          <el-input
              placeholder="重复密码"
              show-password
              type="password"
              v-model="form.repassword"
              autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item id="phone" prop="phone">
          <el-input
              placeholder="手机号"
              type="phone"
              v-model="form.phone"
              autocomplete="off"
          ></el-input>
        </el-form-item>
        <el-form-item id="authcode" prop="authcode">
           <el-input 
            placeholder="邮箱验证码" 
            @search="sendMail"
            v-model="authcode"
            autocomplete="off">   
            <el-button slot="append" v-if="count===0">获取验证码</el-button>
            <el-button slot="append" v-else disabled>{{count}}秒后重试</el-button>
        </el-input>
        </el-form-item>
        <el-form-item class="btn_login">
          <el-button type="primary" @click="register">注&nbsp;&nbsp;册</el-button>
        </el-form-item>
      </el-form>
      <div class="suffix">
        <p @click="toLogin">已有账号？点击这里登录</p>
      </div>
    </div>
  </div>
</template>

<script>
//import qs from "qs";
import Vue from "vue";
export default {
  name: "NewLogin",
  data() {
    return {
      form: {
        username: "",
        email: "",
        password: "",
        phone: "",
        repassword: "",
        authcode: "",
      },
        token: "",
        timer: null,
        count: 0,
        errorLogin: false,
    }
  },
  methods: {
    register() {
      var that = this;
      var regEmail = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
      var regPhone = /^[0-9]{11}$/;
      var errorTip = "";
      if (this.form.username == "") errorTip = "请输入您的用户名";
      else if (this.form.password == "" || this.form.repassword == "")
        errorTip = "请输入您的密码";
      else if (this.form.email == "") errorTip = "请输入您的邮箱";
      else if (this.form.phone == "") errorTip = "请输入您的手机号码";
      else if (this.form.authcode == "") errorTip = "请输入邮箱验证码";
      else if (!regEmail.test(this.form.email)) errorTip = "请输入正确的邮箱";
      else if (!regPhone.test(this.form.phone)) errorTip = "请输入正确的手机号码";
      else if (this.form.password != this.form.repassword)
        errorTip = "两次输入的密码不同";
      if (errorTip != "") {
        this.$message.error(errorTip);
        return;
      }
      Vue.axios({
        method: "post",
        url: "http://10.135.50.67:8080/register",
        data: {
          username: this.form.username,
          password: this.form.password,
          email: this.form.email,
          phone: this.form.phone,
          code: this.form.authcode,
        },
      }).then(function (response) {
        console.log(response.data);
        if (response.data.success == true) {
          that.$message
            .success("注册成功，即将跳转登录页面")
            .then(() => that.$router.push({ path: "/login" }));
        } else {
          that.$message.error(response.data.message);
        }
      });
    },
    toLogin: function () {
      // 跳转注册的路由
      this.$router.push('/newlogin');
    },
    sendMail() {
      var that = this;
      Vue.axios({
        method: "post",
        url: "http://39.106.230.20:8090/code",
        data: {
          username: this.username,
          email: this.email,
        },
      }).then(function (response) {
        console.log(response.data);
        if (response.data.success == true) {
          that.$message.success("验证码已发送");
          that.count = 60;
          that.timer = setInterval(that.startTimer, 1000);
        } else {
          that.$message.error(response.data.message);
        }
      });
    },
    startTimer() {
      this.count -= 1;
      if (this.count == 0) {
        clearInterval(this.timer);
      }
    },
  },
  created() {
    this.$store.state.showNav = false;
  },
  destroyed() {
    this.$store.state.showNav = true;
  },

};
</script>




<style scoped>
#login {
  font-family: 'Noto Serif SC', serif;
  margin-top: 60px;
}
#login >>> .el-input__inner {
  font-family: 'Noto Serif SC', serif;
}
#login .bgbox {
  display: block;
  opacity: 1;
  z-index: -3;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: opacity 1s,transform .25s,filter .25s;
  backface-visibility: hidden;
}
#login .logo {
  cursor: pointer;
  overflow: hidden;
  height: 150px;
}
#login .wrap {
  width: 300px;
  height: 315px;
  padding: 0 25px 0 25px;
  line-height: 40px;
  position: relative;
  display: inline-block;
  background-color: rgba(255, 255, 255, 0.85);
  border-radius: 20px;
}
#login .btn_login {
  margin-top: 25px;
  text-align: center;
}
#login .btn_login button{
  line-height: 10px;
  font-family: 'Noto Serif SC', serif;
  width: 100%;
  height: 38px;
}
#login .suffix {
  font-size:14px;
  line-height:10px;
  color:#999;
  cursor: pointer;
  float:right;
}
</style>