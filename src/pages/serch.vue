<template>
  <div>
      <v-head></v-head>
    <div class="container">
      <div class="row">
        <div class="col main">
          <!-- 活动卡片 -->
          <el-col :span="24" v-for="(item,index) in data" :key="index">
            <el-card shadow="hover" @click.native="todetail(item.id)">
              <el-image :src="item.background_img"></el-image>
              <div class="title">{{item.title}}</div>
              <div class="time">{{item.create_time}}</div>
            </el-card>
          </el-col>
        </div>
      </div>
    </div>
    <v-backtop></v-backtop>
    <v-footer></v-footer>
  </div>
</template>

<script>
import axios from "axios";
import vHead from "../components/head";
import vAdd from "../components/add_top";
import vFooter from "../components/footer";
import vBacktop from "../components/backtop";
export default {
  data() {
    return {
      vl: this.$route.query.vl,
      data:{}
    };
  },
  components: {
    vHead,
    vAdd,
    vFooter,
    vBacktop
  },
  created() {
      axios.post("http://10.12.80.203/api/Article/get_article_list",{
        search:this.vl
      }).then(res=>{
          this.data=res.data.article_list
          console.log(this.data)
      }).catch(err=>{

      })
 
  },
    methods: {
       todetail(id) {
      //跳转闲置页面
       this.$router.push({
        path: "article_detail",
        query: { id: id }
      });
    },
    }
};
</script>
<style scoped lang="scss">
.main {
  margin-top: 50px;
  .el-card {
    cursor: pointer;
    border-radius: 10px;
    padding: 0px;
    margin: 15px;
    .el-image {
      border-radius: 5px;
      width: 100%;
      height: 500px;
    }
    .title {
      font-family: PingFangSC-Medium;
      font-size: 16px;
      color: #333333;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 1;
    }
    .time {
      font-size: 12px;
      color: #999999;
      margin-top: 10px;
      margin-bottom: 10px;
    }
  }
}
</style>