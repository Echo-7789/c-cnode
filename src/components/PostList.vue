<template>
  <div class="PostList">
  <!--  在数据未返回时，显示正在加载的gif-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="正在加载">
    </div>
  <!--  代表我们的主题帖子列表-->
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="topbar">
            <a href="https://cnodejs.org?tab=all">全部</a>
            <a href="https://cnodejs.org?tab=good">精华</a>
            <a href="https://cnodejs.org?tab=share">分享</a>
            <a href="https://cnodejs.org?tab=ask">问答</a>
            <a href="https://cnodejs.org?tab=job">招聘</a>
          </div>
        </li>
        <li v-for="post in posts">
             <!-- 头像-->
          <img :src="post.author.avatar_url" alt="">
             <!--回复、浏览-->
          <span class="allcount">
            <span class="reply_count">{{post.reply_count}}</span>
            /{{post.visit_count}}
          </span>
             <!--帖子的分类-->
          <span :class="[{put_good:(post.good === true),put_top:(post.top === true),
          'topiclist-tab':(post.good !== true && post.top !== true)}]">
            <span>
              {{post | tabFormatter}}
            </span>
          </span>
               <!--标题-->
          <router-link :to="{name:'post_content',params:{id:post.id,name:post.author.loginname}}">
            <span>{{post.title}}</span>
          </router-link>
               <!-- 最终回复时间-->
          <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
        </li>
        <li><pagination @handleList="renderList"></pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import pagination from './Pagination'
export default {
  name: "PostList",
  data(){
    return {
      isLoading:false,
      posts:[], //代表页面的列表数组
      postpage:1
    }
  },
  components:{
    pagination
  },
  methods:{
    getData(){
      this.$http.get('http://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:20}})
        .then(res=>{
          this.isLoading=false;       //加载成功取出动画
          this.posts=res.data.data;
      })
        .catch(function(err){
          //处理返回失败后的问题
          console.log(err)
        })
    },
    renderList(value){
      this.postpage = value;
      this.getData();
    }
  },
  beforeMount(){
    this.isLoading=true;      //加载成功前显示加载动画
    this.getData();           //在页面加载之前获取数据
  }
}
</script>

<style scoped>
.PostList{
  background-color: #e1e1e1;
}
.posts{
  margin-top: 10px;
}
img{
  width:30px;
  height:30px;

}
ul{
  list-style: none;
  width:100%;
  max-width:1344px;
  margin:0 auto;
}
ul li:not(:first-child){
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue","Luxi Sans","DejaVu Sans",Tahoma,"Hiragino Sans GB",STHeiti,sans-serif !important;
  font-weight:400;
  background-color: white;
  color:#333;
  border-top:1px solid #f0f0f0;
}
li:not(first-child):hover{
  background-color: white;
}
li:last-child:hover{
  background: white;
}
li span{
  line-height:30px;
}
.allcount{
  width:70px;
  display:inline-block;
  text-align:center;
  font-size: 12px;
}
.reply_count{
  color:#9e78c0;
  font-size:14px;
}
.put_good, .put_top{
  background-color: #80bd01;
  color:#fff;
  padding:2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius:3px;
  font-size: 12px;
  margin-right:10px;
}
.topiclist-tab{
  background-color: #e5e5e5;
  color:#999;
  padding:2px 4px;
  border-radius:3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius:3px;
  font-size: 12px;
  margin-right: 10px;
}
.topbar{
  height:40px;
  background:#f5f5f5;
}

.topbar a{
  text-decoration: none;
  font-size: 14px;
  color:#80bd01;
  line-height: 40px;
  margin:0 10px;
  cursor:pointer;
}
.topbar a:hover{
  color:whitesmoke;
  background-color: #80bd01;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius:3px;
}
 a{
  text-decoration: none;
}
</style>
