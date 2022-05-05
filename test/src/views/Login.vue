<template>        
    <div class="login">
    <form class="form-box">
      <h1>Login</h1>
      <label><input v-model="username" type="text" placeholder="Username" class="username"></label>
      <label><input v-model="password" type="password" placeholder="Password" class="password"></label>
      <button type="submit" @click="click_login" class="login">Login</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    click_login() {
      window.alert(this.username + this.password);
    }
  }
}
</script>

<style scoped>
.form-box {
  width: 300px;
  padding: 40px;  /* 内边界宽度  */
  position: absolute;  /* 设置为绝对定位，使下方的top和left生效  */
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);  /* 作用见后方描述  */
  background: #90b9e5;  /* 设置背景颜色  */
  text-align: center;  /* 表单中内容居中  */
  border-radius: 10px;
}
.form-box h1 {
  text-transform: uppercase;  /* 将字体全部设置成大写字母  */
}
/* 设置用户名输入框和密码输入框样式 */
.form-box .username, .form-box .password {
  border-radius: 24px;  /* 边框四个角的弧度  */
  border: 2px solid #3498db;  /* 边框厚度  */
  background: none;
  display: block;
  margin: 20px auto;  /* 外边界  */
  text-align: center;
  padding: 14px 10px;  /* 内边界  */
  width: 200px;
  outline: none;
  color: white;     /* 设置输入框中竖线的颜色 */
  transition: 0.25s;   /* 设置元素过渡效果 */
}
/* 设置文本框获得焦点时的样式 */
.form-box .username:focus,.form-box .password:focus{
  border-color: #2ecc71;
}
/* 设置提交按钮的样式 */
.form-box .login{
  border-radius: 24px;
  border: 2px solid #0b95f1;
  background: none;
  display: block;
  margin: 20px auto;
  padding: 14px 40px;
  outline: none;
  transition: 0.25s;
  cursor: pointer;    /* 设置光标的样式 */
}
</style>