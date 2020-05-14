<template>
  <div class="home">
    <div class="top-login">
      <div class="login">
        <div>
          <el-input placeholder="请输入内容" v-model="login.username" clearable></el-input>
        </div>
        <div>
          <el-input placeholder="请输入内容" v-model="login.password" clearable></el-input>
        </div>
        <el-button type="success" @click="goAbout" round>登陆</el-button>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld
  },
  data() {
    return {
      login: {
        username: "",
        password: ""
      }
    };
  },
  methods:{
    goAbout(){
      this.$router.replace('/about')
    }
  }
};
</script>

<style>
.home {
  background-image: url("../static/img/home.jpg");
  background-size: cover;
  height: 100%;
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
}
.top-login {
  margin: 300px 0 0 300px;
}
.login {
  width: 300px;
  /* 外边距上下边距 0   auto 浏览器计算外边距 */
  margin: 10px auto;
}

.login div {
  padding: 10px;
}
</style>
