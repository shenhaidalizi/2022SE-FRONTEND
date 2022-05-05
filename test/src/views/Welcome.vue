<template>
  <div id="login" class="login">
    <img class="bgbox" id="bgbox" alt="" src="../../src/assets/1.jpg">
    <div class="wrap" style="margin-top:150px;margin-left:1100px;">
      <h1>金刚石文档</h1>
      <el-form :model="form" ref="form" class="form">
        <el-form-item class="btn_login">
          <el-button type="primary" @click="toRegister" style="width:100px;">注&nbsp;&nbsp;册</el-button>
          <el-button type="primary" @click="toLogin" style="width:100px;">登&nbsp;&nbsp;录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import qs from "qs";
export default {
  name: "Welcome",
  data() {
    return {
      form: {
        username: '',
        password: '',
      }
    }
  },
  methods: {
    toLogin: function () {
      // 跳转注册的路由
      this.$router.push('/newlogin');
    },
    toRegister: function () {
      // 跳转注册的路由
      this.$router.push('/register');
    }
  }
}
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