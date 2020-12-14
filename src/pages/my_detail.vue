<template>
  <div>
    <v-head></v-head>
    <div class="container">
      <el-tabs v-model="activeName">
        <el-tab-pane label="个人信息" name="first">
          <div>
            <div class="ud-img">
              头像：
              <img  :src="geren.avatar" alt />
                <!-- <img :src="geren.avatar" alt /> -->
              <el-upload
                action="http://10.12.80.203/api/base/upload"
                :show-file-list="false"
                :accept="'image/*'"
                :on-success="handleSuccess"
                
              >
                <el-button type="primary" size="medium">上传头像</el-button>
              </el-upload>
            </div>
            <el-form label-width="80px" :model="formLabelAlign">
              <el-form-item label="用户名">
                <el-input v-model="formLabelAlign.name" :placeholder="geren.name"></el-input>
              </el-form-item>
              <el-form-item label="昵称" width="80px">
                <el-input v-model="formLabelAlign.nickname" :placeholder="geren.nickname"></el-input>
              </el-form-item>
              <el-form-item label="个性签名">
                <el-input v-model="formLabelAlign.signature" :placeholder="geren.signature"></el-input>
              </el-form-item>
              <el-form-item label="性别">
                <el-radio-group v-model="sex" size="medium">
                  <el-radio border @change="getValue()"  v-model="sex"  label="0">男</el-radio>
                  <el-radio border @change="getValue()"  v-model="sex" label="1">女</el-radio>
                </el-radio-group>
              </el-form-item>
              
              <el-form-item>
                <el-button type="primary" @click.native="saveInfo">保存</el-button>
              </el-form-item>
            </el-form>
          </div>
        </el-tab-pane>
        <el-tab-pane label="文章" name="second">
          <div>
            <el-col :span="4" v-for="(item,index) in wz" :key="index">
              <el-card shadow="hover" @click.native="todetail(item.id)">
                <el-image :src="item.background_img"></el-image>
                <div class="title">{{item.title}}</div>
                <div class="time">{{item.create_time}}</div>
              </el-card>
            </el-col>
          </div>
        </el-tab-pane>
        <el-tab-pane label="评论" name="third">
          <div>
            <el-timeline reverse="true">
              <el-timeline-item
                v-for="(item, index) in commentlist"
                :key="index"
                :timestamp="item.time"
              >{{item.content}}</el-timeline-item>
            </el-timeline>
          </div>
        </el-tab-pane>
        <el-tab-pane label="活动" name="fourth">没有更多了</el-tab-pane>
      </el-tabs>
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
      bannerUrl: {},
      sex: {},
      //个人信息
      geren: {},
      commentlist: "",
      wz: "",
      activeName: "first",
      formLabelAlign: {
       
      }
    };
  },
  components: {
    vHead,
    vAdd,
    vFooter,
    vBacktop
  },
  created() {
    //获取发布过的文章和评论

    axios
      .post("http://10.12.80.203/api/user/get_user_data", {
        user_id: localStorage.getItem("userid")
      })
      // .get("http://10.12.80.203/api/user/get_user_data", {
      //   user_id:localStorage.getItem("userid")
      // })
      .then(res => {
        // this.wz = res.data.articles;
        // this.commentlist = res.data.yonghu;
        // this.geren=res.data;
        // this.bannerUrl=res.data.data.avatar
        // console.log(111)
        this.geren = res.data;
        this.sex = JSON.stringify(res.data.sex);
       console.log( 11);
        console.log(res.data);
        this.formLabelAlign=red.data
        console.log(this.geren.avatar);
      })
      .catch(err => {});

      //获取发布文章
      axios
      .post("http://10.12.80.203/api/Article/get_article_list", {
        user_id: localStorage.getItem("userid")
      }).then(res=>{
      
        this.wz=res.data.article_list
      }).catch(err=>{

      })
  },
  methods: {
     //上传成功获取后台返回的链接
    handleSuccess: function(result) {
      this.geren.avatar = result.data[0];
      console.log(222)
   console.log(this.geren.avatar)
    },
     getValue(){
      this.formLabelAlign.sex=this.sex
    },
    todetail(id) {
      //去到个人文章详情页
      this.$router.push({
        path: "article_detail",
        query: { id: id }
      });
    },
    saveInfo() {
      // console.log(this.formLabelAlign.sex);
      //保存个人信息
      axios

        .post("http://10.12.80.203/api/user/edit_user_data", {
         
            // tx: bannerUrl,
            id:localStorage.getItem("userid"),
            nickname: this.formLabelAlign.nickname,
            signature:this.formLabelAlign.signature,
            name: this.formLabelAlign.name,
            sex:this.sex,
            avatar:this.geren.avatar

          
        })
        .then(res => {
          this.$message({
            message: "保存成功！",
            type: "success",
            offset: "80"
          });
        window.localStorage.setItem(
            "tx",
            JSON.stringify(this.geren.avatar)
          );
        })
        .catch(err => {
          this.$message({
            message: "保存出错了！",
            type: "warning",
            offset: "80"
          });
        });
    }
  }
};
</script>
<style scoped lang="scss">
.ud-img {
  margin-left: 10%;
  margin-bottom: 20px;
  img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
  }
  .el-button {
    margin-top: 20px;
    margin-left: 50%;
  }
}
.el-card {
  cursor: pointer;
  border-radius: 10px;
  padding: 0px;
  margin: 15px;
  .el-image {
    border-radius: 5px;
    width: 100%;
    height: 85px;
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
.el-tabs {
  /deep/ .el-tabs__item {
    font-size: 18px;
    color: #999999;
  }
  min-height: 350px;
  padding-top: 100px;
  .el-tab-pane {
    padding-top: 50px;
  }
}
.el-form {
  width: 500px;
}
</style>