<template>
  <div class="header">
    <router-link :to="{name:'root'}">
      <img src="../assets/cnodejs_light.svg" alt="logo">
    </router-link>
    <ul>
      <li><a href="#">首页</a></li>
      <li><a href="https://cnodejs.org/getstart">新手入门</a></li>
      <li><a href="https://cnodejs.org/api">API</a></li>
      <li><a href="https://cnodejs.org/signup">注册</a></li>
      <li><a href="https://cnodejs.org/signin">登录</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
.header{
  background-color:#444;
  height:50px;
}
img{
  max-width:120px;
  margin-left:100px;
  margin-top: 10px;
}
ul{
  list-style: none;
  float:right;
  margin:4px;
}
li{
  display:inline-block;
  padding:10px 15px;
}
a{
  text-decoration: none;
  color:#ccc;
  font-size:14px;
}
</style>
