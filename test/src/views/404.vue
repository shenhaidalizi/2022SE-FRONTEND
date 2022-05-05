<template>
  <div id="login" class="login">
    <img class="bgbox" id="bgbox" alt="" src="../../src/assets/moyu.jpg">
    <h1>这里没有你想要的东西，只有一只想要摸鱼的小猫</h1>
    <el-button @click="toHome" key="console" type="primary" size="large">
                返回工作台
    </el-button>
    
  </div>
</template>

<script>

  export default {
    name: '404',
    components: {
    },
    data() {
      return {
      }
    },
    methods:{
        toHome() {
            this.current = "home";
            this.$router.push({ path: "/" });
        },
    }
  }
</script>